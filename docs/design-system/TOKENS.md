# Token Design — ACT Strive

**Status:** Mengikuti keputusan S4 (draft final — freeze bersama brief)  
**Logo:** [`assets/act-strive-128.svg`](./assets/act-strive-128.svg) (sumber legacy `act-strive/public/act-srive-128.svg`)

---

## 1. Brand colors (dari logo legacy)

| Token | Hex | Pemakaian |
|-------|-----|-----------|
| `brand.teal` | `#024e45` | Wordmark, sidebar accent |
| `brand.tealDark` | `#004d45` | Gradient mark, hover primary |
| `brand.gold` | `#e3b329` | Accent gradient (mark), highlight sparingly |
| `brand.gradient` | `#e3b329` → `#004d45` | Logo mark saja; **bukan** background penuh UI |

### Ant Design `ConfigProvider` — Light theme

```typescript
{
  token: {
    colorPrimary: '#024e45',
    colorLink: '#024e45',
    colorSuccess: '#389e0d',
    colorWarning: '#d48806',
    colorError: '#cf1322',
    borderRadius: 6,
    fontSize: 14,
  },
  components: {
    Layout: {
      siderBg: '#024e45', // atau #001529 jika kontras menu putih — MVP: teal brand
      triggerBg: '#004d45',
    },
  },
}
```

### Dark theme

- Pakai Ant Design **`theme.algorithm: theme.darkAlgorithm`**
- Override: `colorPrimary: '#2a9d8f'` (teal lebih terang untuk kontras dark)
- Sidebar dark: `#141414` dengan logo full color
- **Wajib uji kontras** status Tag di dark mode

Toggle: user menu **Light / Dark / System** (system = `prefers-color-scheme`).

---

## 2. Status dokumen (semantic)

| Status | Light Tag | Dark Tag |
|--------|-----------|----------|
| DRAFT | `default` | `default` |
| PENDING_APPROVAL | `warning` | `warning` |
| APPROVED | `success` bordered | `success` |
| REJECTED | `error` | `error` |
| POSTED / ACTIVE | `success` | `success` |
| CANCELLED | `default` + strikethrough | same |
| CLOSED | `purple` | `purple` |

Label **bilingual** via i18n key `status.{enum}.label`.

---

## 3. Spacing & layout

| Token | Value |
|-------|-------|
| `pagePadding` | 24px |
| `cardGap` | 16px |
| `formMaxWidth.simple` | 720px |
| `tableDefaultPageSize` | 20 |
| `tablePageSizeOptions` | 10, 20, 50, 100 |

---

## 4. Typography

- Font stack: Ant Design default (system UI) v1 MVP
- Wordmark logo: SVG (bukan render font LINE Seed JP)
- Monospace nomor dokumen: optional `font-family: ui-monospace`

---

## 5. Logo usage

| Konteks | Asset |
|---------|--------|
| Sidebar collapsed | Icon mark saja (crop dari SVG atau varian 32px) |
| Sidebar expanded | Mark + teks "act strive" |
| Login | `act-strive-128.svg` max-height 64px |
| Favicon | Export 32px dari mark (task implementasi) |

Jangan ubah warna mark di light/dark kecuali varian monochrome untuk favicon.
