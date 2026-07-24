# 2. Referensi Proyek Lama

Dokumen ini merangkum aset dan modul dari proyek legacy sebagai baseline rebuild.

---

## 2.1 Stack legacy

### Frontend (`act-strive`)

| Komponen | Teknologi |
|----------|-----------|
| Framework | React 18 + Vite 7 |
| Routing | React Router 7 |
| UI | Ant Design 6 + MUI (campuran) |
| State | Redux Toolkit + React Query |
| Styling | Tailwind CSS 3 |
| Validasi | Zod |
| Real-time | WebSocket client |
| PDF | jsPDF |

**Struktur modul FE** (`src/modules/`):

| Modul | Status di legacy | Catatan |
|-------|------------------|---------|
| `setting` | ✅ Luas | Company, IAM, workflow, risk, ecommerce |
| `auth` | ✅ | Login, guard |
| `accounting` | ✅ | COA, journal, journal rules, ledger |
| `finance` | ✅ | Currency, exchange rate, TOP |
| `inventory` | 🟡 Partial | Master warehouse; transaksi direncanakan |
| `sales` | 🟡 Partial | Routing ada |
| `purchase` | 🟡 Partial | Routing ada |
| `inquiry` | 🟡 Partial | UI komponen |
| `manufacturing` | 🟡 Partial | Routing; BOM belum di BE |
| `warehouse` | 🟡 | Master data |
| `customer` | ✅ | Master + approval snapshot |
| `vendor` | ✅ | Master + approval snapshot |
| `fixedAsset` | 🟡 | Halaman depreciation |
| `hr` | 🟡 | Struktur dasar |
| `dashboard` | 🟡 | Placeholder |

**Komponen reusable penting:**

- `components/erp-table` — tabel ERP dengan filter, sort, export
- `components/pdfPreview` — preview dokumen
- Workflow builder + JSON Logic builder (`json-logic-js`)

### Backend (`act-strive-api`)

| Komponen | Teknologi |
|----------|-----------|
| Framework | Express 5 |
| ORM | Prisma 7 + PostgreSQL |
| Cache | Redis (ioredis) |
| Real-time | ws |
| Validasi | Zod |
| Auth | JWT + bcrypt + refresh token |

**Modul BE** (`src/modules/`):

| Modul | Fungsi |
|-------|--------|
| `auth`, `user`, `userOnboarding` | Autentikasi & user lifecycle |
| `companies`, `companyGroup`, `companySetting`, `companyUnit`, `companyCurrency` | Tenant & cabang |
| `orgRole`, `orgLevel`, `userOrgRole`, `dynamicActor` | IAM & approval actors |
| `workflowDefinition`, `approvalEngine` | Approval engine baru |
| `baseRisk`, `riskRule`, `riskFactor`, `risk` | Risk scoring |
| `chartOfAccount`, `glJournal`, `journalRuleDefinition`, `journalRuleEngine` | Accounting |
| `inventoryAccountMapping` | Mapping COA inventory |
| `warehouse`, `warehouseLocation` | Gudang |
| `customer`, `vendor` | Master mitra bisnis |
| `currency`, `exchangeRate`, `top` | Finance master |
| `country`, `ecommerce` | Referensi |
| `notification` | Notifikasi |
| `seeder` | Data seed |

---

## 2.2 Database legacy (Prisma)

Schema legacy: **~70 model**, **~97 KB** (`prisma/schema.prisma`).

### Domain clusters

```
IAM & Tenant
├── User, RefreshToken, UserDevice, UserAccessCompany
├── CompanyGroup, Company, CompanySetting, CompanyCurrency
├── Unit, OrgRole, OrgLevel, UserOrgRole, UserHierarchy, DynamicActor

Finance & GL
├── Currency, ExchangeRate, ExchangeRateHistories, ExchangeRateChangeRequest
├── ChartOfAccount, ChartOfAccountSnapshot
├── GlJournalHeader, GlJournalLine
├── JournalHeader, JournalLine (legacy/subledger)
├── JournalRuleDefinition, AccountingPeriod, PeriodCloseJobRun
├── TermPayment, TermEarlyPaymentDiscount, TermPenalty, TermAccounting

Inventory & Warehouse
├── Warehouse, WarehouseSnapshot, WarehouseLocation, WarehouseLocationSnapshot
├── InventoryAccountMapping

Procurement Chain (Inquiry integrated)
├── Inquiry → ProcurementRequirement → Rfq → Quotation
├── CustomerPo → PurchaseOrder → DeliveryOrder
├── Invoice → Payment → PaymentAllocation

Master Partners
├── Customer (+ Bank, Document, Snapshot)
├── Vendor (+ Bank, Document, Tax, Company/Individual, Snapshot)

Governance
├── WorkflowDefinition, WorkflowStep, WorkflowRule
├── ApprovalRequest, ApprovalRequestStep, ApprovalActionLog
├── Approval (legacy template)
├── BaseRisk, RiskRule, RiskFactor, RiskResult, RiskResultDetail

Other
├── Notification, SeederData, ListEcommerce, VendorEcommerce
```

### Model procurement (sudah ada di schema)

Legacy sudah mendefinisikan rantai **Inquiry → Payment** dengan `currencyCode` per dokumen — ini menjadi fondasi modul inquiry terintegrasi sales/purchasing.

---

## 2.3 Dokumentasi legacy yang diwarisi

Folder `act-strive-api/docs/` berisi desain matang:

| Dokumen | Relevansi rebuild |
|---------|-------------------|
| `APPROVAL_FLOW.md` | Port langsung ke NestJS approval module |
| `COA_*` | Struktur COA hierarki 5 level |
| `GL_JOURNAL_API.md`, `GL_MULTICURRENCY_JOURNAL_SCHEMES.md` | Spesifikasi jurnal multi-currency |
| `JOURNAL_*` | Period state machine, journal engine |
| `INVENTORY_*` | Sprint plan inventory + GL integration |
| `MANUFACTURING_BOM_*` | Desain BOM multi-level |

**Rekomendasi:** Salin/adapt dokumen domain spesifik ke `docs/domain/` di repo baru pada fase implementasi modul terkait.

---

## 2.4 Gap analysis (legacy → target)

| Area | Legacy | Gap / Aksi rebuild |
|------|--------|-------------------|
| Inventory transaksi | Belum di API | Implementasi P0 per `INVENTORY_BASE_SPRINT_PLAN.md` |
| Manufacturing BOM/WO | Desain saja | Implementasi model + service baru |
| Fixed Asset | FE partial | Model Prisma + depreciation run belum ada |
| HRIS | FE skeleton | Employee, attendance, leave — model baru |
| IAM permission (RABAC) | Placeholder UI | Implementasi permission matrix |
| Industry IMPA/ATA | Belum ada | Extension item master + katalog |
| Branch inter-company | Company multi | Transfer pricing / IC elimination — fase 2 |
| FE stack | Vite SPA | Migrasi ke Next.js App Router |
| BE structure | Express flat | NestJS modules dengan boundary jelas |

---

## 2.5 Fitur legacy yang wajib dipertahankan

1. **Approval Engine baru** (workflow_definitions → steps → requests) — jangan kembali ke legacy template saja.
2. **Risk engine** terintegrasi approval (rule expression JSON Logic).
3. **Snapshot pattern** untuk Customer/Vendor/Warehouse (audit + approval apply).
4. **Exchange rate change request** dengan approval dan emergency override log.
5. **GL Journal Plan B** dengan `debit_base` / `credit_base` + transaction currency trail.
6. **Dynamic Actor** untuk resolver approver non-statis.
7. **WebSocket notification** per company dengan target entity id.

---

## 2.6 Technical debt yang tidak diwarisi

- Campuran MUI + Ant Design di FE → standarisasi satu design system (Ant Design atau shadcn)
- Express route tanpa module boundary ketat → NestJS module isolation
- Permission comment-out di routes → RBAC aktif sejak awal
- Legacy Approval Template → deprecate setelah migrasi data
