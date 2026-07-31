# Design System Brief — ACT Strive ERP

**Topic ID:** S4  
**Status:** **`FREEZE`** (ADR-001)  
**Versi:** v1.0  
**Tracker:** [`../DISCUSSION_AGENDA.md`](../DISCUSSION_AGENDA.md)

Keputusan stakeholder **2026-07-25** tercatat di §3. Wireframe MVP: [`wireframes/`](./wireframes/). Token: [`TOKENS.md`](./TOKENS.md). Komponen: [`COMPONENTS.md`](./COMPONENTS.md).

---

## 1. Tujuan design system

1. **Satu bahasa visual** untuk ERP multi-modul (setting, finance, inventory, inquiry, dll.).
2. **Pola interaksi repeatable** — user belajar sekali, dipakai di semua dokumen transaksi.
3. **Efisiensi build** — komponen shared (`ErpTable`, status chip, approval timeline) konsisten di Next.js.
4. **Konsistensi governance** — status Draft → Posted tampil sama di semua modul.
5. **MVP first** — wireframe struktural dulu; **perbaikan UX/UI menyusul** setelah MVP fungsional.

---

## 2. Batasan & non-goals (v1)

| In scope v1 | Out of scope v1 |
|-------------|-----------------|
| Web desktop-first (1280px+) | Mobile native |
| **Ant Design 6 saja** | MUI / library lain |
| **Light + Dark mode** | — |
| **Bilingual ID + EN** | Locale > 2 |
| Logo **legacy** (`act-strive-128.svg`) | Rebrand |
| Pola list + detail + form | Visual polish / ilustrasi custom |
| **`ErpTable` reusable** (sort, search, pagination server-side) | Material React Table |
| Wireframe R1/R2 (low-fi) | Figma hi-fi (post-MVP) |

---

## 3. Keputusan produk (FREEZE candidate)

| # | Keputusan | Keputusan final | Status |
|---|-----------|-----------------|--------|
| D1 | UI library | **Ant Design 6** + `@ant-design/nextjs-registry`; **tanpa MUI** | ✅ |
| D1b | Tabel list modul | **`ErpTable`** wrapper Ant Table — wajib semua list; fitur penuh sort/search/filter/pagination server-side — lihat [`COMPONENTS.md`](./COMPONENTS.md) | ✅ |
| D2 | CSS utility | **Tailwind CSS** layout; Ant untuk komponen | ✅ |
| D3 | Bahasa UI | **Bilingual** (`id` default, `en` fallback); glosarium ERP (PO, COA, …) tetap English | ✅ |
| D4 | Dark mode | **Ya v1** — toggle Light/Dark/System; token dark di [`TOKENS.md`](./TOKENS.md) | ✅ |
| D5 | Density tabel | **Middle** default | ✅ |
| D6 | Font | System UI stack; wordmark via **SVG logo** | ✅ |
| D7 | Icon | **Ant Design Icons** | ✅ |
| D8 | Tanggal & angka | `dayjs`; format dari **Company Setting** | ✅ |
| D9 | Notifikasi | Ant notification + badge; WebSocket | ✅ |
| D10 | PDF / print | Modal preview + print CSS (implementasi per module spec) | ✅ |
| D11 | Logo | **Ikuti logo lama** — [`assets/act-strive-128.svg`](./assets/act-strive-128.svg) | ✅ |
| D12 | Wireframe | **Dibuat untuk MVP** (R1 list, R2 detail); UX polish **setelah MVP** | ✅ |

---

## 4. Brand & token

Lihat [`TOKENS.md`](./TOKENS.md).

Ringkas:

- Primary: `#024e45` (teal logo)
- Accent mark gradient: `#e3b329` → `#004d45`
- Status dokumen: mapping global Tag semantic (§ TOKENS)

---

## 5. Pola UX platform

Tidak berubah dari draft; wireframe mengikat layout:

| Pola | Wireframe |
|------|-----------|
| List + ErpTable | [`wireframes/R1-list-purchase-order.md`](./wireframes/R1-list-purchase-order.md) |
| Detail + tabs + approval | [`wireframes/R2-detail-purchase-order.md`](./wireframes/R2-detail-purchase-order.md) |

### Keputusan shell (MVP default)

| Item | Keputusan |
|------|-----------|
| Sidebar | Collapsible permanent; grouped menu (Purchasing, Finance, …) |
| Header | Cabang, **locale ID\|EN**, **theme toggle**, notifikasi, user |
| Breadcrumb | Wajib |

### Post-MVP UX (explicit backlog)

- Empty state ilustrasi
- Workflow builder visual polish
- Column picker persist advanced
- Mobile responsive pass

---

## 6. Komponen shared

| ID | Nama | MVP |
|----|------|-----|
| C1 | AppShell | ✅ |
| C2 | **ErpTable** | ✅ **wajib** |
| C3 | DocumentStatusTag | ✅ |
| C4 | DocumentHeader | ✅ |
| C5 | DocumentTabs | ✅ |
| C6 | PageFilterBar | ✅ |
| C7 | ApprovalTimeline | ✅ |
| C8 | ApprovalInbox | ✅ |
| C9 | MoneyInput | ✅ |
| C10 | ConfirmLeaveModal | ✅ |
| C11 | PermissionGate | ✅ (permission string final di **S3**) |
| C12 | EmptyState | ✅ minimal teks |

Spesifikasi C2: [`COMPONENTS.md`](./COMPONENTS.md).

---

## 7. Halaman referensi

| Ref | Dokumen | Status |
|-----|---------|--------|
| R1 | [`wireframes/R1-list-purchase-order.md`](./wireframes/R1-list-purchase-order.md) | ✅ |
| R2 | [`wireframes/R2-detail-purchase-order.md`](./wireframes/R2-detail-purchase-order.md) | ✅ |
| Review stakeholder | — | ☐ konfirmasi formal |

---

## 8. Integrasi Next.js

| Topik | Keputusan |
|-------|-----------|
| i18n | **`next-intl`** (rekomendasi) — route atau cookie locale |
| Theme | `ConfigProvider` + `theme.darkAlgorithm` + token brand |
| React Query | List data untuk ErpTable |

---

## 9. Aksesibilitas baseline

Kontras teks WCAG AA (termasuk **dark mode**), focus visible, status dengan teks label.

---

## 10. Exit criteria — S4 FREEZE

- [x] D1–D12 keputusan final
- [x] Token brand dari logo legacy (`TOKENS.md`)
- [x] Pola list + detail (wireframe R1/R2)
- [x] ErpTable spec lengkap
- [x] Bilingual + dark mode policy
- [x] **Sign-off stakeholder** (nama + tanggal di bawah)

---

## 11. Open questions — resolved

| # | Keputusan |
|---|-----------|
| Sidebar grouped | Ya, MVP default |
| Table | **ErpTable** on Ant Design Table only |
| Workflow builder UI | **v1.1** (post-MVP UX) |
| JSON Logic editor | **v1.1** |

---

## 12. Setelah freeze S4

1. ~~TOKENS.md, COMPONENTS.md~~ — sudah draft
2. Lanjut **S1 MVP charter** atau **S3 IAM** (untuk PermissionGate)
3. Module spec mengacu R1/R2
4. Post-MVP: pass UX/UI polish, Figma, empty states

---

## Sign-off FREEZE

- Topic ID: S4  
- Tanggal freeze: **2026-07-25**  
- Disetujui oleh: **Ali Shoddiqien**  
- Versi dokumen: **v1.0**  
- ADR: [`../decisions/ADR-001-freeze-design-system-s4.md`](../decisions/ADR-001-freeze-design-system-s4.md)
