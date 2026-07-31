# ADR-001: Freeze Design System (S4)

- **Status:** Accepted
- **Tanggal:** 2026-07-25
- **Disetujui oleh:** Ali Shoddiqien

## Konteks

Pra-coding memerlukan design system dan pola UI platform yang mengikat sebelum implementasi Next.js.

## Keputusan

1. **Ant Design 6** sebagai satu-satunya UI library.
2. Komponen **`ErpTable`** wajib untuk semua halaman list (sort, search, filter, pagination server-side, resize kolom).
3. UI **bilingual** (Indonesia default, English).
4. **Dark mode** v1 (Light / Dark / System).
5. Logo dan primary color mengikuti asset legacy (`act-strive-128.svg`, primary `#024e45`).
6. Wireframe MVP **R1** (list) dan **R2** (detail + approval); polish UX/UI **setelah MVP fungsional**.

## Dampak

- Dokumen mengikat: `docs/design-system/DESIGN_SYSTEM_BRIEF.md` v1.0, `TOKENS.md`, `COMPONENTS.md`, wireframes R1/R2.
- Perubahan hanya via ADR amendment.
- Module spec dan implementasi UI mengacu pola R1/R2.

## Referensi

- [`../design-system/DESIGN_SYSTEM_BRIEF.md`](../design-system/DESIGN_SYSTEM_BRIEF.md)
