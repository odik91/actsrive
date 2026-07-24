# 1. Visi dan Ruang Lingkup

## 1.1 Visi produk

ACT Strive adalah **ERP hybrid** yang dapat dipakai lintas industri:

- **General** — trading, jasa, distribusi
- **Manufaktur** — BOM multi-level, work order, costing
- **Marine** — pengadaan suku cadang kapal dengan katalog **IMPA**
- **Aviasi** — pengadaan suku cadang pesawat dengan katalog **ATA**

Aplikasi dirancang sebagai **SaaS single-tenant**: setiap deployment melayani **satu tenant** (satu grup perusahaan), dengan dukungan **multi cabang** di dalam tenant tersebut.

---

## 1.2 Tujuan rebuild

| Aspek | Proyek lama | Target rebuild |
|-------|-------------|----------------|
| Frontend | React SPA (Vite) | **Next.js** (App Router, SSR/SSG where needed) |
| Backend | Express monolith | **NestJS** modular (domain modules, DI, guards) |
| Database | PostgreSQL + Prisma | **Tetap PostgreSQL + Prisma** (migrasi schema terkontrol) |
| Real-time | WebSocket manual | NestJS Gateway + Redis pub/sub |
| Auth | JWT + cookie custom | NestJS Passport/JWT + refresh token rotation |
| Approval | Engine custom (sudah matang) | **Port** ke NestJS service layer |
| Accounting | GL journal engine (Plan B) | **First-class module** sejak awal |

Rebuild **bukan** copy-paste; fokus pada:

1. Arsitektur modular yang konsisten
2. Domain model yang sudah terbukti di legacy (Prisma schema ~70 model)
3. UX yang lebih terstruktur (Next.js layout per modul)
4. Extensibility per industri via **Industry Profile**

---

## 1.3 Prinsip desain

### Hybrid ERP

Satu platform menangani:

- **Operasional** — inventory, sales, purchasing, manufacturing
- **Keuangan** — GL, multi-currency, AR/AP, fixed asset
- **People** — HRIS, organisasi, approval hierarchy
- **Governance** — approval engine, audit trail, period close

Modul diaktifkan per **Industry Profile** + **Company Setting**, bukan hard-coded per klien.

### Multi-currency by design

Setiap transaksi komersial (quotation, PO, invoice, payment) menyimpan:

- `currencyCode` — mata uang transaksi
- `exchangeRateId` — rujukan kurs saat posting
- `amountBase` — nilai dalam mata uang pelaporan (functional currency)

Detail skema: [`05-MULTI_CURRENCY_AND_FINANCE.md`](./05-MULTI_CURRENCY_AND_FINANCE.md).

### Multi cabang

Cabang = entitas **`Company`** di bawah **`CompanyGroup`** (tenant). Data operasional (warehouse, transaksi, COA) di-scope per `companyId`. User dapat akses multi-company via `UserAccessCompany`.

Detail: [`06-MULTI_BRANCH_AND_TENANCY.md`](./06-MULTI_BRANCH_AND_TENANCY.md).

### Approval everywhere

Transaksi master data dan operasional yang berisiko tinggi melewati **Approval Engine** (workflow definition → steps → request → action log). Risk engine menentukan workflow branch.

Detail: [`07-APPROVAL_AND_RISK_ENGINE.md`](./07-APPROVAL_AND_RISK_ENGINE.md).

---

## 1.4 Ruang lingkup v1 (MVP+)

### In scope

- IAM, Setting, Cabang (Company/Unit)
- Approval Engine + Risk Engine
- Master: Customer, Vendor, Item, Warehouse
- Inventory base (receipt, issue, adjustment, transfer)
- Inquiry → RFQ → Quotation → Customer PO → PO → DO → Invoice → Payment
- Sales & Purchasing (terintegrasi inquiry untuk perusahaan pengadaan)
- Accounting: COA, GL Journal, Period, Journal Rule Engine
- Finance: Currency, Exchange Rate (dengan approval)
- Fixed Asset: tangible, intangible, prepaid amortization
- HRIS dasar (employee master, org structure)
- Manufacturing: BOM multi-level, work order (fase awal)
- Industry profile: General + Marine (IMPA) + Aviasi (ATA) — katalog & field extension
- Notification (in-app + email)
- Dashboard operasional

### Out of scope v1 (fase berikutnya)

- Payroll full (BPJS, PPh 21 run)
- MRP / APS advanced
- E-commerce marketplace integration (baca: listing platform vendor — legacy sudah ada stub)
- Mobile native app
- Multi-tenant shared database (SaaS multi-tenant) — **sengaja di luar scope**; model = single tenant per deployment

---

## 1.5 Stakeholder & persona

| Persona | Kebutuhan utama |
|---------|-----------------|
| Admin IT / Super Admin | IAM, company setup, workflow, industry profile |
| Finance Manager | COA, jurnal, period close, multi-currency, fixed asset |
| Procurement Officer | Inquiry, RFQ, vendor, PO, approval |
| Sales Officer | Quotation, customer PO, delivery, invoice |
| Warehouse Staff | GR, GI, stock opname, transfer |
| Production Planner | BOM, WO, material issue, FG receipt |
| HR Admin | Employee, department, attendance (fase awal) |
| Approver | Inbox approval, eskalasi, SLA |

---

## 1.6 Keberhasilan (success metrics)

1. Satu tenant dapat menjalankan **≥ 2 cabang** dengan transaksi terpisah dan konsolidasi laporan.
2. Transaksi USD/EUR → posting GL base IDR dengan realized/unrealized FX benar.
3. BOM 3 level+ dapat di-explode ke material issue tanpa siklus.
4. Approval workflow configurable per `entityType` tanpa deploy ulang.
5. Industry Marine: pencarian item by IMPA code; Aviasi: by ATA chapter.
