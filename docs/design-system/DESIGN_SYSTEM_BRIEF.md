# Design System Brief — ACT Strive ERP

**Topic ID:** S4  
**Status pembahasan:** `SEDANG_DIBAHAS` (belum FREEZE)  
**Tracker:** [`../DISCUSSION_AGENDA.md`](../DISCUSSION_AGENDA.md)

Dokumen ini mendefinisikan **tujuan, batasan, dan keputusan** design system sebelum coding UI. Isi bertanda **TBD** harus diselesaikan sebelum freeze.

---

## 1. Tujuan design system

1. **Satu bahasa visual** untuk ERP multi-modul (setting, finance, inventory, inquiry, dll.).
2. **Pola interaksi repeatable** — user belajar sekali, dipakai di semua dokumen transaksi.
3. **Efisiensi build** — komponen shared (ERP table, status chip, approval timeline) mengurangi duplikasi di Next.js.
4. **Konsistensi governance** — status Draft/Submitted/Approved/Posted tampil sama di semua modul.
5. **Migrasi dari legacy** — mengacu pola yang sudah dikenal di `act-strive` (Ant Design, ERP table) tanpa membawa technical debt campuran MUI.

---

## 2. Batasan & non-goals (v1 brief)

| In scope v1 | Out of scope v1 |
|-------------|-----------------|
| Web desktop-first (1280px+) | Mobile native |
| Ant Design sebagai base (jika disetujui) | Multi UI library |
| Light theme (+ dark TBD) | Custom design language dari nol tanpa component library |
| Pola list + detail + form master | Marketing landing page |
| Komponen ERP platform (table, doc header, tabs) | Storybook lengkap semua modul (bisa fase 2) |
| Aksesibilitas **baseline** (kontras, focus visible) | Sertifikasi WCAG penuh |

---

## 3. Keputusan produk (checklist freeze)

Centang dan isi sebelum S4 → **FREEZE**.

| # | Keputusan | Rekomendasi | Keputusan final | Status |
|---|-----------|-------------|-----------------|--------|
| D1 | UI component library | **Ant Design 6** + `@ant-design/nextjs-registry` | TBD | ☐ |
| D2 | CSS utility | **Tailwind CSS** untuk layout/spacing; Ant Design untuk komponen | TBD | ☐ |
| D3 | Bahasa UI v1 | **Indonesia** (istilah ERP English: PO, GRN, COA) | TBD | ☐ |
| D4 | Dark mode | **Tidak v1** (hanya light); siapkan token agar v2 mudah | TBD | ☐ |
| D5 | Density tabel default | **Middle**; opsi compact di user preference (fase 2) | TBD | ☐ |
| D6 | Font | Ant Design default **Inter/system**; heading brand TBD | TBD | ☐ |
| D7 | Icon set | **Ant Design Icons** + custom module icons minimal | TBD | ☐ |
| D8 | Tanggal & angka | `dayjs`; format dari **Company Setting** (separator, date format) | TBD | ☐ |
| D9 | Notifikasi | In-app **Ant Design** notification + badge; WS realtime | TBD | ☐ |
| D10 | PDF / print | Pola preview modal + print stylesheet (jsPDF/react-pdf TBD saat module spec) | TBD | ☐ |

---

## 4. Brand & token (draft — revisi saat workshop)

### 4.1 Warna brand

| Token | Usage | Nilai sementara | Final |
|-------|--------|-----------------|-------|
| `colorPrimary` | Primary button, link | TBD (legacy: sesuaikan logo ACT Strive) | ☐ |
| `colorSuccess` | Posted, Approved | `#52c41a` (Ant default) atau custom | ☐ |
| `colorWarning` | Pending, Submit | `#faad14` | ☐ |
| `colorError` | Rejected, Error | `#ff4d4f` | ☐ |
| `colorInfo` | Draft, Info | `#1677ff` | ☐ |

### 4.2 Warna status dokumen ERP (wajib konsisten)

Semua modul memakai **Tag/Badge** dengan mapping enum yang sama:

| Status bisnis | Warna semantic | Contoh modul |
|---------------|----------------|--------------|
| `DRAFT` | default / grey | PO, Invoice, Vendor snapshot |
| `PENDING_APPROVAL` | warning | Semua entitas approval |
| `APPROVED` | success (outline) | Pre-post |
| `REJECTED` | error | |
| `POSTED` / `ACTIVE` | success (solid) | GL posted, master active |
| `CANCELLED` / `VOID` | default + strikethrough label | |
| `CLOSED` | purple atau geekblue | Period, WO completed |

**TBD:** Nama enum global `DocumentStatus` vs per-modul — rekomendasi: **core enum + alias label** per modul.

### 4.3 Spacing & layout

| Area | Aturan |
|------|--------|
| App shell | Sidebar kiri + header (company switcher, user, notifikasi) |
| Content padding | 24px desktop; 16px tablet |
| Max width form | `720px` (master sederhana) / full width (transaksi dengan line items) |
| Breadcrumb | Wajib di halaman dashboard kecuali modal |

### 4.4 Typography

| Level | Penggunaan |
|-------|------------|
| Page title | `Title` level 3 — nomor dokumen + status tag |
| Section | `Title` level 5 — tab content, card header |
| Body | 14px default Ant Design |
| Table cell | 14px; angka kanan-align; monospace optional untuk nomor dokumen |

---

## 5. Pola UX platform (wajib)

### 5.1 App shell

```
┌──────────────────────────────────────────────────────────┐
│ Logo │ Module nav...          │ Cabang ▼ │ 🔔 │ User ▼ │
├──────────┬───────────────────────────────────────────────┤
│ Sidebar  │ Breadcrumb                                    │
│ (module) │ ┌─────────────────────────────────────────┐ │
│          │ │ Page title              [Primary actions] │ │
│          │ │ Tabs / filters                            │ │
│          │ │ Content                                   │ │
│          │ └─────────────────────────────────────────┘ │
└──────────┴───────────────────────────────────────────────┘
```

**Keputusan TBD:**

- Sidebar: collapsible permanent vs drawer mobile (v1 desktop-only: permanent collapsible)
- Module grouping: Settings | Operations | Finance | HR (TBD)

### 5.2 Pola halaman — List (index)

Wajib ada:

- Title + tombol **Create** (permission-gated)
- Filter bar (search, status, date range, cabang jika multi-view)
- **ERP Table**: sort, pagination, column visibility (port dari legacy `erp-table`)
- Row actions: View, Edit (jika draft), Delete (jika draft + policy)
- Empty state ilustrasi + CTA create
- Bulk actions (fase 2 kecuali dibutuhkan v1)

### 5.3 Pola halaman — Detail dokumen / master

Layout **header sticky** + **tabs**:

| Tab | Isi |
|-----|-----|
| Overview / Detail | Field header + line items table |
| Approval | Timeline steps, approver, notes (jika applicable) |
| Attachments | Upload list (jika applicable) |
| Audit / History | Created, updated, snapshot version |
| Related | Link dokumen turunan (PO → GRN) |

**Action bar** (kanan atas, konsisten urutan):

1. Secondary: Print / Export
2. Secondary: Cancel edit
3. Primary: Save (draft)
4. Primary: Submit (trigger approval)
5. Destructive: Reject / Cancel doc ( gated )

Status **Posted** → form read-only; hanya aksi reversal/credit note sesuai module spec (TBD per modul).

### 5.4 Pola form

- **Master sederhana:** single column + section card
- **Transaksi dengan lines:** header form + editable table (inline add row)
- Validasi: inline field error + summary on submit
- Unsaved changes: confirm leave dialog
- Multi-currency: tampilkan **currency selector** + **kurs readonly** + kolom **setara base** (read-only atau side-by-side)

### 5.5 Approval inbox (global)

- Menu **Tasks / Approval** dengan badge count
- List: entity type, nomor, requestor, SLA, step current
- Detail drawer atau full page dengan approve/reject + **note wajib on reject**

### 5.6 Feedback & errors

| Situasi | Pola |
|---------|------|
| Save success | Toast success |
| Validation | Field + optional Alert |
| API error | Message + correlation id (support) |
| Permission denied | Empty state + contact admin |
| Loading | Skeleton table / spin on button |

### 5.7 Realtime

- WebSocket event → invalidate React Query keys scoped `companyId` + `targetEntityId` (legacy lesson)
- Toast optional untuk approval assigned to me

---

## 6. Komponen shared wajib (platform)

Prioritas implementasi setelah freeze (urutan):

| ID | Komponen | Deskripsi | Legacy ref |
|----|----------|-----------|------------|
| C1 | `AppShell` | Layout sidebar + header + company switcher | — |
| C2 | `ErpTable` | Data grid + filter + export hook | `act-strive/components/erp-table` |
| C3 | `DocumentStatusTag` | Mapping enum → warna | — |
| C4 | `DocumentHeader` | Title, number, status, action buttons | — |
| C5 | `DocumentTabs` | Tab layout detail | — |
| C6 | `PageFilterBar` | Search + filters | — |
| C7 | `ApprovalTimeline` | Steps + logs | FE approval views |
| C8 | `ApprovalInbox` | List pending tasks | — |
| C9 | `MoneyInput` | Amount + currency + base preview | finance modules |
| C10 | `ConfirmLeaveModal` | Unsaved guard | — |
| C11 | `PermissionGate` | Hide/disable by permission | — |
| C12 | `EmptyState` | Illustration + text | — |

**TBD setelah S3 IAM:** integrasi `PermissionGate` dengan permission string final.

---

## 7. Halaman referensi (wajib untuk freeze)

Dua halaman **wireframe atau Figma** low-fi/high-fi:

| Ref | Halaman | Alasan |
|-----|---------|--------|
| **R1** | List transaksi (contoh: Purchase Order atau Inquiry) | Validates ErpTable + filter + status |
| **R2** | Detail dokumen + tab Approval + line items | Validates pola dokumen ERP utama |

**Status:**

- [ ] R1 wireframe
- [ ] R2 wireframe
- [ ] Review stakeholder

Boleh mock data statis; belum perlu API.

---

## 8. Integrasi Next.js + Ant Design

| Topik | Rekomendasi |
|-------|-------------|
| App Router | Route groups `(auth)`, `(dashboard)` |
| Ant Design SSR | `@ant-design/nextjs-registry` + `StyleProvider` |
| Theme | `ConfigProvider` theme token di root layout |
| React Query | Provider di dashboard layout |
| Forms | Ant Design Form + Zod resolver (TBD lib: `@ant-design/zod` or custom) |

---

## 9. Aksesibilitas (baseline v1)

- Kontras teks minimal WCAG AA untuk body text
- Focus ring visible pada keyboard navigation
- Icon-only buttons wajib `aria-label`
- Status tidak hanya warna (selalu ada teks label)

---

## 10. Exit criteria — S4 FREEZE

Semua harus **✅** sebelum ubah status di [`DISCUSSION_AGENDA.md`](../DISCUSSION_AGENDA.md) ke **FREEZE**:

- [ ] D1–D10 keputusan final terisi (tabel §3)
- [ ] Warna primary brand + status dokumen disetujui (§4)
- [ ] Pola list + detail + approval disetujui (§5)
- [ ] Daftar komponen C1–C12 disetujui (revisi boleh via amendment)
- [ ] R1 + R2 wireframe selesai dan direview (§7)
- [ ] Sign-off stakeholder (§ template di agenda)

---

## 11. Open questions (update log sesi)

1. **Sidebar:** full module list vs grouped — preferensi user?
2. **Table:** Material React Table legacy vs Ant Design Table murni — rekomendasi **Ant Table Pro** atau port erp-table custom?
3. **Workflow builder UI:** port visual builder legacy ke Next.js — masuk design system v1 atau v1.1?
4. **JSON Logic editor** untuk risk/workflow — komponen code editor vs visual (legacy punya builder)?

---

## 12. Langkah setelah freeze S4

1. Turunkan token ke `TOKENS.md` (nilai final).
2. Dokumentasi pola ke `PATTERNS.md`.
3. Spesifikasi props ke `COMPONENTS.md`.
4. Lanjut **S3 IAM** (PermissionGate) atau **S1 MVP** paralel.
5. Baru **module spec** dengan mockup mengacu R1/R2.

---

## Sign-off FREEZE

*(Kosongkan sampai exit criteria terpenuhi)*

- Topic ID: S4  
- Tanggal freeze: —  
- Disetujui oleh: —  
- Versi dokumen: v0.1 draft  
