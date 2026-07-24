# Dokumentasi Perencanaan ACT Strive (Rebuild)

Dokumen ini merencanakan **rebuild** aplikasi ERP hybrid ACT Strive dari proyek lama:

| Proyek lama | Peran | Stack lama | Stack baru |
|-------------|-------|------------|------------|
| `act-strive` | Frontend | React + Vite + Ant Design | **Next.js** |
| `act-strive-api` | Backend | Express + Prisma + PostgreSQL | **NestJS** + Prisma |

Referensi kode dan desain lama ada di:
- `C:\Users\Adek\Documents\Project\act-strive`
- `C:\Users\Adek\Documents\Project\act-strive-api`

---

## Indeks dokumen

| File | Isi singkat |
|------|-------------|
| [`01-VISION_AND_SCOPE.md`](./01-VISION_AND_SCOPE.md) | Visi produk, ruang lingkup, prinsip desain |
| [`02-LEGACY_REFERENCE.md`](./02-LEGACY_REFERENCE.md) | Inventaris modul & aset yang diwarisi dari proyek lama |
| [`03-ARCHITECTURE.md`](./03-ARCHITECTURE.md) | Arsitektur sistem, layer, integrasi antar modul |
| [`04-MODULE_CATALOG.md`](./04-MODULE_CATALOG.md) | Katalog modul lengkap (wajib + tambahan) |
| [`05-MULTI_CURRENCY_AND_FINANCE.md`](./05-MULTI_CURRENCY_AND_FINANCE.md) | Multi-currency, kurs, GL, jurnal |
| [`06-MULTI_BRANCH_AND_TENANCY.md`](./06-MULTI_BRANCH_AND_TENANCY.md) | Multi cabang, SaaS single-tenant |
| [`07-APPROVAL_AND_RISK_ENGINE.md`](./07-APPROVAL_AND_RISK_ENGINE.md) | Approval engine, workflow, risk scoring |
| [`08-MANUFACTURING.md`](./08-MANUFACTURING.md) | BOM multi-level, work order, WIP |
| [`09-INDUSTRY_PROFILES.md`](./09-INDUSTRY_PROFILES.md) | Profil industri: General, Manufaktur, Marine (IMPA), Aviasi (ATA) |
| [`10-TECH_STACK_AND_STRUCTURE.md`](./10-TECH_STACK_AND_STRUCTURE.md) | Stack teknis, struktur monorepo, konvensi |
| [`11-IMPLEMENTATION_ROADMAP.md`](./11-IMPLEMENTATION_ROADMAP.md) | Fase implementasi, prioritas, exit criteria |
| [`12-FIXED_ASSET.md`](./12-FIXED_ASSET.md) | Aset berwujud/tidak berwujud, prepaid, depresiasi/amortisasi |

### Pra-coding (agenda & desain)

| File | Isi singkat |
|------|-------------|
| [`DISCUSSION_AGENDA.md`](./DISCUSSION_AGENDA.md) | **Tracker pembahasan**, status freeze, checkpoint |
| [`design-system/DESIGN_SYSTEM_BRIEF.md`](./design-system/DESIGN_SYSTEM_BRIEF.md) | Brief design system (**S4 — sedang dibahas**) |
| [`modules/MODULE_SPEC_TEMPLATE.md`](./modules/MODULE_SPEC_TEMPLATE.md) | Template spesifikasi modul (S7) |
| [`decisions/`](./decisions/) | ADR setelah freeze |
| [`policies/`](./policies/) | MVP, data scope, IAM, finance policy |

---

## Konvensi penulisan

1. **Judul file:** `HURUF_BESAR_SNAKE_CASE.md` dengan prefix nomor urut.
2. **Bahasa:** Narasi **Bahasa Indonesia**; istilah teknis/produk dalam **English** bila sudah dipakai di kode (enum, endpoint, nama modul).
3. **Referensi legacy:** Sebut path modul lama bila relevan untuk migrasi fitur.
4. **Status dokumen:** Draft perencanaan — revisi seiring keputusan stakeholder.

---

## Sinkronisasi

- Commit folder `docs/` ke Git repository `actstrive`.
- Dokumen desain detail domain (COA, jurnal, inventory) tetap merujuk ke `act-strive-api/docs/` sebagai referensi historis hingga dimigrasikan ke repo ini.
