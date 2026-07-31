# Product Charter — MVP Scope (S1)

**Topic ID:** S1  
**Status:** `SEDANG_DIBAHAS`  
**Tracker:** [`../DISCUSSION_AGENDA.md`](../DISCUSSION_AGENDA.md)  
**Prasyarat FREEZE:** Keputusan §4–§6 + skenario UAT §7 disetujui + sign-off

Dokumen ini **mengikat** ruang lingkup MVP rebuild ACT Strive. Referensi visi: [`../01-VISION_AND_SCOPE.md`](../01-VISION_AND_SCOPE.md) (tidak menggantikan charter ini).

---

## 1. Tujuan MVP

**MVP Release 1.0** = tenant production-ready untuk perusahaan **pengadaan barang & jasa** (inquiry-driven) dengan **multi cabang**, **multi-currency**, **approval**, **inventory**, dan **pembukuan GL**, deploy **single-tenant SaaS**.

**Bukan tujuan MVP:** fitur lengkap semua industri sekaligus, payroll, MRP, mobile app, multi-tenant shared DB.

---

## 2. Persona v1 (wajib dilayani)

| ID | Persona | Peran MVP |
|----|---------|-----------|
| P1 | Super Admin | Setup company, IAM, workflow, kurs |
| P2 | Finance Manager | COA, jurnal, period, multi-currency, rekonsiliasi dasar |
| P3 | Procurement Officer | Inquiry → RFQ → PO vendor |
| P4 | Sales / Account Officer | Quotation → Customer PO → invoice customer |
| P5 | Warehouse Staff | GR/GI, adjustment, transfer |
| P6 | Approver | Inbox approval, approve/reject |
| P7 | Branch User | Switch cabang, data terisolasi |

**Out persona MVP:** Production Planner (full WO), HR Payroll — fase **Release 1.1** kecuali disetujui masuk 1.0 (§4).

---

## 3. Profil industri MVP

| Profil | Release 1.0 | Release 1.1 | Catatan |
|--------|-------------|-------------|---------|
| **General** (trading/pengadaan) | ✅ **Wajib** | — | Default semua company |
| **Marine (IMPA)** | ☐ TBD | ☐ TBD | Inquiry + field vessel; katalog IMPA |
| **Aviation (ATA)** | ☐ TBD | ☐ TBD | Serial/batch; katalog ATA |
| **Manufacturing** | ☐ TBD | ☐ TBD | BOM/WO |

**Rekomendasi tim dokumen:** Release **1.0 = General + alur inquiry lengkap**; Marine IMPA di **1.1** (90 hari pasca go-live); Aviasi & Manufaktur **1.2**.

**Keputusan final:** ☐ Setuju rekomendasi ☐ Ubah (catat di §9)

---

## 4. Modul — Release 1.0 (MVP Go-Live)

### 4.1 Platform & governance

| Modul | Scope 1.0 | Catatan |
|-------|-----------|---------|
| Settings | Company group, company, company setting, country | |
| Cabang | Multi `Company`, switch context | ≥ 2 cabang UAT |
| IAM | User, login, UserAccessCompany, org role/level/unit, user-org-role | Permission matrix detail → **S3** |
| Approval Engine | Workflow definition, runtime request, action log | Port legacy |
| Risk Engine | Base risk, rule, factor, evaluasi | Terintegrasi approval |
| Notification | In-app + email stub + WebSocket | |

### 4.2 Finance & accounting

| Modul | Scope 1.0 |
|-------|-----------|
| Currency & company currency | ✅ |
| Exchange rate + change request + approval | ✅ |
| Term of payment (TOP) | ✅ |
| COA hierarki + approval snapshot | ✅ |
| GL Journal (posting manual + otomatis dari hook) | ✅ |
| Accounting period + soft/hard close | ✅ |
| Journal rule engine | ✅ Minimal (hook inventory + AR/AP) |

### 4.3 Master & inventory

| Modul | Scope 1.0 |
|-------|-----------|
| Customer / Vendor | Master + bank + approval snapshot |
| Item / SKU | Master + UOM + costing method flag |
| Warehouse + location | Master + approval |
| Inventory | Receipt, issue, adjustment, transfer, stock card |
| Inventory ↔ GL | Posting + rekonsiliasi dasar |

### 4.4 Procurement & sales (inquiry-centric)

| Modul | Scope 1.0 |
|-------|-----------|
| Inquiry | Create, requirements |
| RFQ | Ke vendor |
| Quotation | Vendor quote + customer quote (multi-currency) |
| Customer PO | ✅ |
| Purchase Order | ✅ (wireframe R1/R2) |
| GRN / receipt link | ✅ ke inventory |
| Delivery Order | ✅ |
| Invoice AR/AP | ✅ |
| Payment + allocation | ✅ + realized FX |

### 4.5 Lainnya Release 1.0

| Modul | Scope 1.0 |
|-------|-----------|
| Dashboard | KPI minimal (open PO, approval pending, stock alert) |
| Audit log | ☐ TBD — rekomendasi **ya** untuk master & posting |
| UX | Pola **S4 FREEZE**; polish visual post-MVP |

---

## 5. Release 1.1 (pasca go-live, target +90 hari)

| Modul | Scope |
|-------|--------|
| HRIS | Employee master + link Unit |
| Fixed Asset | Register, depresiasi/amortisasi, GL |
| Marine | IMPA catalog + mapping item + field inquiry |
| Manufacturing | BOM multi-level + WO dasar |
| Cash & bank recon | Optional |

**Keputusan:** ☐ Setuju pemisahan 1.0 / 1.1 ☐ Gabungkan modul ke 1.0: ___

---

## 6. Explicit out of scope (semua release awal)

- Payroll BPJS / PPh 21 run
- MRP / APS
- E-commerce marketplace sync (legacy stub)
- Mobile native
- Multi-tenant satu database banyak klien
- Konsolidasi group elimination
- E-faktur DJP
- Partner portal vendor/customer

---

## 7. Skenario UAT MVP Release 1.0

Setiap skenario = **go / no-go** saat go-live.

| # | Skenario | Persona | Bukti lulus |
|---|----------|---------|-------------|
| U1 | Login → switch cabang → data cabang A ≠ B | P1, P7 | Screenshot + query id |
| U2 | Buat vendor → submit → approve → active | P3, P6 | Approval log |
| U3 | Request ubah kurs → approve → dipakai PO USD | P2, P6 | Rate id on PO |
| U4 | Inquiry → RFQ → quotation vendor → customer quotation → customer PO | P3, P4 | Rantai nomor dokumen |
| U5 | Customer PO → PO vendor → GRN → stok bertambah | P3, P5 | Stock card |
| U6 | DO → Invoice customer USD → payment → realized FX journal | P2, P4 | GL balanced |
| U7 | PO → invoice vendor → payment AP | P2, P3 | GL balanced |
| U8 | Inventory issue + adjustment → jurnal + period close block | P5, P2 | Posting rejected saat closed |
| U9 | Workflow PO amount > threshold → 2 step approval | P6 | approval_steps |
| U10 | Dua cabang: PO cabang 1 tidak tampil di cabang 2 | P7 | ErpTable list |

**Tambahan opsional:** ☐ U11 BOM/WO jika masuk 1.0

---

## 8. KPI keberhasilan MVP (measurable)

1. **U1–U10** lulus di staging tenant demo.
2. **≥ 2 cabang** aktif dengan COA & gudang terpisah.
3. **≥ 1** transaksi USD end-to-end dengan jurnal base IDR benar.
4. **0** double-post GL (idempotency) pada retry posting.
5. Waktu setup tenant baru (seed + super admin): target **≤ 1 hari kerja** (proses, bukan automasi wajib v1).

---

## 9. Batasan teknis MVP (align arsitektur)

| Aspek | MVP |
|-------|-----|
| FE | Next.js + S4 FREEZE |
| BE | NestJS + Prisma |
| Deploy | Single-tenant per customer |
| Bahasa | Bilingual UI |
| Coding | Setelah **S1 FREEZE** + **S2** data scope + **S3** IAM minimal + module spec wave 1 |

**Module spec wave 1 (setelah S1 freeze):** Settings/IAM → Finance/Accounting → Master → Inventory → Inquiry/PO chain.

---

## 10. Exit criteria — S1 FREEZE

- [ ] §3 Profil industri Release 1.0 vs 1.1 disetujui
- [ ] §4 Modul 1.0 disetujui (tidak ada modul wajib bisnis yang hilang)
- [ ] §5 Release 1.1 disetujui
- [ ] §7 Skenario UAT final (10 skenario, edit jika perlu)
- [ ] §8 KPI disetujui
- [ ] Sign-off stakeholder

---

## 11. Pertanyaan untuk Ali Shoddiqien (jawab untuk freeze S1)

1. **Marine IMPA** masuk **Release 1.0** atau **1.1**?
2. **Manufacturing BOM/WO** masuk **1.0**, **1.1**, atau **1.2**?
3. **Fixed Asset + HRIS** — setuju **1.1**?
4. **Audit log** wajib di **1.0**?
5. Ada **modul wajib** yang belum tercantum di §4?

---

## Sign-off FREEZE

- Topic ID: S1  
- Tanggal freeze: —  
- Disetujui oleh: —  
- Versi dokumen: v0.1 draft  
