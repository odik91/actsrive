# Agenda Pembahasan & Status Freeze

**Terakhir diperbarui:** 2026-07-25  
**Fokus aktif:** **S4 — Design System** (siap freeze, menunggu sign-off)  
**Berikutnya:** **S1 Product charter MVP** (disarankan)

---

## Cara pakai

1. Topic **`SEDANG_DIBAHAS`** → revisi sampai exit criteria ✅.
2. Topic **`FREEZE`** → hanya ubah lewat **`AMENDMENT`** (`docs/decisions/ADR-XXX.md`).
3. Update checkpoint & log sesi setelah setiap meeting.

---

## Aturan freeze vs lanjut

| Status | Arti |
|--------|------|
| `SEDANG_DIBAHAS` | Belum final — checklist exit criteria belum 100% atau belum sign-off |
| `FREEZE` | Keputusan mengikat |
| `REFERENSI` | Dokumen `01–12` — bukan charter MVP final |

**Freeze S4** = checklist §10 brief ✅ + sign-off nama/tanggal.

---

## Daftar agenda

| ID | Topik | Dokumen | Status | Freeze? |
|----|-------|---------|--------|---------|
| S0 | Perencanaan awal | `docs/01`–`12` | `REFERENSI` | — |
| S1 | Product charter MVP | `policies/MVP_SCOPE.md` | `BELUM_DIMULAI` | ⏳ |
| S2 | Data scope cabang | `policies/DATA_SCOPE.md` | `BELUM_DIMULAI` | ⏳ |
| S3 | IAM & permission | `policies/IAM_PERMISSIONS.md` | `BELUM_DIMULAI` | ⏳ |
| **S4** | **Design system** | [`design-system/`](./design-system/) | **`SEDANG_DIBAHAS`** → siap freeze | ⏳ → 🔒 setelah sign-off |
| S5 | Approval registry | `policies/APPROVAL_REGISTRY.md` | `BELUM_DIMULAI` | ⏳ |
| S6 | Finance policy | `policies/FINANCE_POLICY.md` | `BELUM_DIMULAI` | ⏳ |
| S7 | Module spec | `modules/*.md` | `BELUM_DIMULAI` | ⏳ per modul |
| S8 | Industry profile | `09` + policy | `BELUM_DIMULAI` | ⏳ |

---

## Checkpoint — sampai mana

### S4 Design System (2026-07-25)

- [x] Ant Design 6 saja + **ErpTable reusable** (sort, search, pagination, filter server-side)
- [x] Bahasa **bilingual** (ID + EN)
- [x] **Dark mode** ya (Light/Dark/System)
- [x] Logo **legacy** → `design-system/assets/act-strive-128.svg`
- [x] Token warna (`TOKENS.md`)
- [x] Wireframe **R1** list PO
- [x] Wireframe **R2** detail + approval tab
- [x] Kebijakan **UX polish setelah MVP**
- [x] Sign-off formal → status **FREEZE**

### Antrian

- [ ] S1 MVP charter
- [ ] S2 Data scope
- [ ] S3 IAM
- [ ] S5, S6, S7, S8

---

## Log sesi

| Tanggal | ID | Ringkasan |
|---------|-----|-----------|
| 2026-07-24 | S4 | Brief awal + tracker |
| 2026-07-25 | S4 | Keputusan: Ant Design 6, ErpTable wajib, bilingual, dark mode, logo legacy, wireframe R1/R2 MVP; TOKENS + COMPONENTS + wireframes ditulis |

---

## Keputusan S4 (ringkas)

| Topik | Keputusan |
|-------|-----------|
| UI | Ant Design 6 only |
| Table | `ErpTable` — konsisten di semua modul list |
| Bahasa | Bilingual ID/EN |
| Theme | Dark mode v1 |
| Logo | Legacy SVG |
| Wireframe | R1/R2 low-fi; polish UX later |

---

## Langkah Anda

1. Review wireframe: [`R1`](./design-system/wireframes/R1-list-purchase-order.md), [`R2`](./design-system/wireframes/R2-detail-purchase-order.md)
2. Jika OK → balas **"setuju freeze S4"** + nama/peran
3. Kita lanjut **S1 Product charter MVP** (scope & skenario UAT)

---

## Pertanyaan terbuka (bukan blocker freeze S4)

1. Sidebar background: **teal brand** (`#024e45`) vs **dark navy** (`#001529`) — wireframe assume teal; bisa amend
2. Export CSV: halaman aktif saja vs semua data — tentukan di module spec PO
