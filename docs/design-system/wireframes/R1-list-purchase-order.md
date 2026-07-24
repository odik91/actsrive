# Wireframe R1 — List Purchase Order / Daftar PO

**Ref:** R1  
**Tujuan MVP:** Pola list + `ErpTable` + bilingual + filter  
**Fidelity:** Low-fi (struktur & komponen, bukan visual final)

---

## Layout

```
┌─────────────────────────────────────────────────────────────────────────────┐
│ [Logo act strive]                    Cabang: HQ Jakarta ▼  🌐 ID|EN  🌙  🔔3  User ▼ │
├──────────────┬──────────────────────────────────────────────────────────────┤
│ ▼ Purchasing │  Beranda / Purchasing / Purchase Order                        │
│   Inquiry    │  ───────────────────────────────────────────────────────────  │
│   PO  ◀──────│  Purchase Order / Pesanan Pembelian          [ + Buat PO ]     │
│   GRN        │                                                               │
│ ▼ Settings   │  ┌─ Filter bar ─────────────────────────────────────────────┐ │
│              │  │ Status: [All ▼]  Vendor: [________]  Date: [__] - [__]   │ │
│              │  │                                    [Reset] [Terapkan]    │ │
│              │  └──────────────────────────────────────────────────────────┘ │
│              │  ┌─ ErpTable toolbar ───────────────────────────────────────┐ │
│              │  │ [↻ Refresh]  [⬇ Export]              Columns ▼           │ │
│              │  └──────────────────────────────────────────────────────────┘ │
│              │  ┌──────────────────────────────────────────────────────────┐ │
│              │  │ No PO ▼🔍    │ Vendor ▼🔍 │ Tanggal ↕ │ Total ↕ │ Status │ ⋮ │ │
│              │  ├──────────────┼────────────┼───────────┼─────────┼────────┼───┤ │
│              │  │ PO-2026-001  │ PT ABC     │ 01/07/26  │ USD 5k  │ [Draft]│ 👁│ │
│              │  │ PO-2026-002  │ PT XYZ     │ 03/07/26  │ IDR 80M │ [Pending]│ 👁│ │
│              │  │ ...          │            │           │         │        │   │ │
│              │  └──────────────────────────────────────────────────────────┘ │
│              │  Showing 1-20 of 134          [ < ] 1 2 3 ... [ > ]  [20/page ▼] │
└──────────────┴──────────────────────────────────────────────────────────────┘
```

---

## Anotasi interaksi

| Elemen | Perilaku MVP |
|--------|----------------|
| `▼🔍` di header kolom | Search per kolom (ErpTable `searchable`) |
| `↕` | Sort server-side |
| Status filter header | `filterable` + enum status |
| `[ + Buat PO ]` | `PermissionGate` → route create |
| Row `👁` / klik row | Navigate ke R2 detail |
| `🌐 ID\|EN` | Toggle locale (semua label halaman) |
| `🌙` | Toggle light/dark |
| Export | CSV dari data page atau full (TBD module spec) |

---

## Copy bilingual (contoh)

| Key | ID | EN |
|-----|----|----|
| page.title | Pesanan Pembelian | Purchase Orders |
| action.create | Buat PO | Create PO |
| col.number | No. PO | PO Number |
| col.vendor | Vendor | Vendor |
| col.date | Tanggal | Date |
| col.total | Total | Total |
| col.status | Status | Status |
| status.DRAFT.label | Draft | Draft |

---

## Out of scope wireframe ini (post-MVP UX)

- Illustrasi empty state custom
- Bulk select / bulk approve
- Kanban view
- Mobile drawer nav
