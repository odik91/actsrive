# Wireframe R2 — Detail PO + Lines + Tab Approval

**Ref:** R2  
**Tujuan MVP:** Pola dokumen transaksi + tabs + approval timeline  
**Contoh:** PO-2026-002 status `PENDING_APPROVAL`

---

## Layout — Tab Overview

```
┌─────────────────────────────────────────────────────────────────────────────┐
│ [App shell — sama R1]                                                        │
├──────────────┬──────────────────────────────────────────────────────────────┤
│ (sidebar)    │  Beranda / Purchasing / PO / PO-2026-002                      │
│              │  ───────────────────────────────────────────────────────────  │
│              │  PO-2026-002   [Menunggu Persetujuan / Pending Approval]       │
│              │                    [Cetak] [Export]  [Simpan] [Ajukan] (disabled)│
│              │  ┌ Tabs ──────────────────────────────────────────────────────┐│
│              │  │ [Ringkasan / Overview] │ Baris / Lines │ Persetujuan / Approval │ Lampiran │ Riwayat ││
│              │  └─────────────────────────────────────────────────────────────┘│
│              │  ┌─ Card: Header ─────────────────────────────────────────────┐ │
│              │  │ Vendor        PT XYZ (readonly jika pending)               │ │
│              │  │ Tanggal PO    03/07/2026                                   │ │
│              │  │ Mata uang     USD    Kurs    16.500 (readonly)             │ │
│              │  │ Term payment  Net 30                                       │ │
│              │  │ Referensi     RFQ-2026-010 (link)                          │ │
│              │  └────────────────────────────────────────────────────────────┘ │
│              │  ┌─ Line items (Ant Table, bukan ErpTable — inline edit) ────┐ │
│              │  │ # │ Item / SKU │ Qty │ UOM │ Harga │ Pajak │ Subtotal     │ │
│              │  │ 1 │ Bolt M8    │ 100 │ PCS │ 2.00  │ 11%   │ 200.00       │ │
│              │  │ 2 │ ...        │     │     │       │       │              │ │
│              │  │   │ [ + Tambah baris ] (disabled if not draft)             │ │
│              │  ├─────────────────────────────────────────────────────────────┤ │
│              │  │                    Subtotal USD 5.000,00                     │ │
│              │  │                    Pajak    USD   550,00                     │ │
│              │  │                    Total    USD 5.550,00  (≈ IDR 91.575.000)│ │
│              │  └────────────────────────────────────────────────────────────┘ │
└──────────────┴──────────────────────────────────────────────────────────────┘
```

---

## Layout — Tab Approval / Persetujuan

```
│              │  ┌─ ApprovalTimeline ─────────────────────────────────────────┐ │
│              │  │ Step 1  Manager Purchasing     ✓ Disetujui / Approved      │ │
│              │  │         by: Budi — 04/07/2026  "OK"                          │ │
│              │  │ Step 2  Finance Manager        ● Menunggu / Pending  (SLA 2d)│ │
│              │  │         [ Setujui / Approve ]  [ Tolak / Reject ]            │ │
│              │  │         Catatan / Note: [________________________]           │ │
│              │  │ Step 3  Director               ○ Belum / Waiting             │ │
│              │  └────────────────────────────────────────────────────────────┘ │
│              │  ┌─ Action log (table kecil) ─────────────────────────────────┐ │
│              │  │ Waktu │ User │ Aksi │ Catatan                                │ │
│              │  └────────────────────────────────────────────────────────────┘ │
```

---

## Aturan MVP (read-only vs edit)

| Status dokumen | Form header | Line edit | Tombol primary |
|----------------|-------------|-----------|----------------|
| DRAFT | Edit | Edit | Simpan, Ajukan |
| PENDING_APPROVAL | Readonly | Readonly | Approve/Reject (approver saja) |
| APPROVED | Readonly | Readonly | Post / Create GRN (module spec) |
| POSTED | Readonly | Readonly | Limited (cancel policy TBD) |

---

## Komponen design system yang divisualkan

- `DocumentHeader` + `DocumentStatusTag`
- `DocumentTabs`
- `ApprovalTimeline` + `ApprovalActionBar`
- Line table: Ant `Table` (bukan ErpTable)
- `MoneyInput` pattern di header (currency + base preview)

---

## Bilingual tab labels

| Tab ID | ID | EN |
|--------|----|----|
| overview | Ringkasan | Overview |
| lines | Baris | Lines |
| approval | Persetujuan | Approval |
| attachments | Lampiran | Attachments |
| history | Riwayat | History |

---

## Post-MVP UX (explicitly later)

- Sticky summary card
- Inline comment thread
- Split view PDF PO
- Optimized mobile
