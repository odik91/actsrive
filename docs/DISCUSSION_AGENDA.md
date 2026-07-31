# Agenda Pembahasan & Status Freeze

**Terakhir diperbarui:** 2026-07-25  
**Fokus aktif:** **S1 — Product charter MVP**  
**Selesai:** **S4 Design System** 🔒

---

## Cara pakai

1. Topic **`SEDANG_DIBAHAS`** → revisi sampai exit criteria ✅ + sign-off.
2. Topic **`FREEZE`** → perubahan hanya lewat **ADR** di `docs/decisions/`.
3. Update checkpoint & log sesi setelah setiap meeting.

---

## Daftar agenda

| ID | Topik | Dokumen | Status | Freeze? |
|----|-------|---------|--------|---------|
| S0 | Perencanaan awal | `docs/01`–`12` | `REFERENSI` | — |
| **S1** | **Product charter MVP** | [`policies/MVP_SCOPE.md`](./policies/MVP_SCOPE.md) | **`SEDANG_DIBAHAS`** | ⏳ |
| S2 | Data scope cabang | `policies/DATA_SCOPE.md` | `BELUM_DIMULAI` | ⏳ |
| S3 | IAM & permission | `policies/IAM_PERMISSIONS.md` | `BELUM_DIMULAI` | ⏳ |
| S4 | Design system | [`design-system/`](./design-system/) | **`FREEZE`** | 🔒 ADR-001 |
| S5 | Approval registry | `policies/APPROVAL_REGISTRY.md` | `BELUM_DIMULAI` | ⏳ |
| S6 | Finance policy | `policies/FINANCE_POLICY.md` | `BELUM_DIMULAI` | ⏳ |
| S7 | Module spec | `modules/*.md` | `BELUM_DIMULAI` | ⏳ |
| S8 | Industry profile | `09` + policy | `BELUM_DIMULAI` | ⏳ |

---

## Checkpoint

### S4 — FREEZE ✅

- Sign-off: **Ali Shoddiqien**, 2026-07-25  
- ADR: [`decisions/ADR-001-freeze-design-system-s4.md`](./decisions/ADR-001-freeze-design-system-s4.md)

### S1 — sedang dibahas

- [x] Draft charter [`policies/MVP_SCOPE.md`](./policies/MVP_SCOPE.md)
- [ ] Jawaban §11 (industri & release 1.0 vs 1.1)
- [ ] Konfirmasi modul §4–§5
- [ ] Konfirmasi UAT U1–U10
- [ ] Sign-off → **FREEZE S1**

### Antrian

- [ ] S2 Data scope (bisa paralel setelah draft S1)
- [ ] S3 IAM
- [ ] S5, S6, S7

---

## Log sesi

| Tanggal | ID | Ringkasan |
|---------|-----|-----------|
| 2026-07-24 | S4 | Brief awal + tracker |
| 2026-07-25 | S4 | Keputusan UI; wireframe R1/R2 |
| 2026-07-25 | S4 | **FREEZE** — Ali Shoddiqien; ADR-001 |
| 2026-07-25 | S1 | Draft MVP_SCOPE Release 1.0 / 1.1 + UAT U1–U10 |

---

## Langkah berikutnya (Anda)

Jawab **§11** di [`MVP_SCOPE.md`](./policies/MVP_SCOPE.md) (5 pertanyaan), atau balas singkat:

- IMPA: 1.0 / 1.1  
- Manufacturing: 1.0 / 1.1 / 1.2  
- Fixed Asset + HRIS: setuju 1.1?  
- Audit log 1.0: ya/tidak  
- Modul wajib lain?

Setelah disepakati → **"setuju freeze S1"** → lanjut **S2 Data scope cabang**.
