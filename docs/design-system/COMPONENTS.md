# Komponen Platform — Spesifikasi MVP

Fokus **fungsi konsisten**; polish visual menyusul setelah MVP.

---

## C2 — `ErpTable` (wajib, semua modul list)

**Keputusan:** Satu wrapper **Ant Design `Table`** — port konsep legacy `ErpTable`, **bukan** Material React Table.

### Tujuan

Sort, search per kolom, filter enum, pagination server-side, lebar kolom persist — **identik perilaku** di setiap modul.

### API (konsep TypeScript)

```typescript
interface ErpTableProps<T extends { id: string }> {
  /** Unique key localStorage: column widths + optional column visibility */
  storageKey: string;
  columns: ErpColumn<T>[];
  /** Server-side data via React Query */
  query: (params: ErpTableQueryParams) => {
    data?: { records: T[]; totalRecords: number };
    isLoading: boolean;
    isRefetching: boolean;
  };
  rowKey?: keyof T;
  onRowClick?: (record: T) => void;
  toolbarExtra?: React.ReactNode;
  exportConfig?: { filename: string; mapRow: (r: T) => Record<string, unknown> };
}

type ErpColumn<T> = ColumnType<T> & {
  searchable?: boolean;
  searchPlaceholder?: string;
  filterable?: boolean;
  filters?: { text: string; value: string | number | boolean }[];
  /** default hidden in column picker */
  defaultHidden?: boolean;
};
```

### Fitur wajib v1

| Fitur | Perilaku |
|-------|----------|
| **Pagination** | Server-side; sync `page`, `limit` ke URL query (shareable link) |
| **Sort** | Server-side single column; `field` + `order` asc/desc |
| **Search per kolom** | Kolom `searchable: true` → filter dropdown Ant Design |
| **Filter enum** | Kolom `filterable` + `filters` → multi-select |
| **Loading** | `loading` prop; skeleton row optional |
| **Empty** | Slot `EmptyState` + i18n |
| **Resize column** | Drag header (persist `localStorage` per `storageKey`) |
| **Column visibility** | Dropdown "Columns" — persist optional v1.1 |
| **Refresh** | Tombol reload query |
| **Density** | `middle` default (Ant Table size) |
| **Sticky header** | `scroll.y` untuk list panjang |
| **Row actions** | Kolom `actions` fixed right |

### Kontrak API backend (list endpoint)

Query params standar (seluruh modul):

```
?page=1&limit=20&field=createdAt&order=desc&search[number]=PO-&filters[status]=DRAFT
```

Response:

```json
{
  "data": {
    "records": [],
    "totalRecords": 0
  }
}
```

### Larangan

- Jangan pakai `Table` Ant Design langsung di halaman modul tanpa `ErpTable` (kecuali tabel kecil inline ≤5 baris di form).
- Jangan client-side only pagination untuk data master/transaksi skala ERP.

### Legacy reference

`act-strive/src/components/erp-table/` — port ke `apps/web/components/erp-table/`.

---

## C1 — `AppShell`

- Logo dari `assets/act-strive-128.svg`
- Company switcher, locale switcher **ID | EN**, theme toggle **Light | Dark**
- Notification badge, user menu

---

## C3 — `DocumentStatusTag`

Props: `status`, `module?` → i18n label ID/EN + warna semantic (TOKENS.md).

---

## C4–C12

Lihat [`DESIGN_SYSTEM_BRIEF.md`](./DESIGN_SYSTEM_BRIEF.md) §6; implementasi mengikuti wireframe R1/R2.

---

## i18n (bilingual)

| Library | `next-intl` (rekomendasi) atau `react-i18next` |
| Struktur key | `{module}.{page}.{field}` dan `status.{enum}.label` |
| Default locale | `id` |
| Fallback | `en` |
| ERP terms | PO, GRN, COA, RFQ — **tidak diterjemahkan** (glosarium konsisten) |

Locale switcher di header; persist `localStorage` + user profile (fase 2).
