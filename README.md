# Officer Field Reference + Medical Kit Inventory — Self-Contained Build

Every page is a **single file** with all CSS, the PIN gate, and the kit data inlined.
No `assets/` folder, no external requests (except Google Fonts, which fall back to system
fonts if offline). Works opened directly (`file://`), written to a tag, or hosted anywhere.

```
index.html            Field reference — fully self-contained
inventory.html        Medical kit inventory (hub + per-kit form) — fully self-contained
tools/pin-hash.html   Generate a new PIN hash (local, offline)
.nojekyll  404.html   GitHub Pages helpers
```

Keep `index.html` and `inventory.html` in the **same folder** so the links between them and
the `inventory.html?kit=…` deep links resolve.

## PIN (default 2468) — now lives in TWO files
Because the gate is inlined into each page, the hash exists in **both** `index.html` and
`inventory.html`. To change the PIN:
1. Open `tools/pin-hash.html`, type the new 4-digit PIN, copy the digest.
2. Find `GATE_PIN_HASH = "…"` in **`index.html`** and paste the new digest.
3. Do the same in **`inventory.html`**. (Search both files for `GATE_PIN_HASH`.)

Reminder: a browser-side PIN is deterrence, not real security — anyone who views source can
brute-force four digits. For sensitive content, put the site behind Cloudflare Access / Netlify
password and keep the PIN as a fast second layer.

## Editing content
- **Field reference:** the `EMERGENCY` and `SECTIONS` arrays near the bottom of `index.html`.
  Add your dispatch number in `EMERGENCY` (`href:'tel:5125551234'`). Entries tagged `SET` are
  placeholders awaiting your links/numbers.
- **Kits:** the `KITS` object inside `inventory.html`. `last` = last recorded count; set `req`
  (par level) on an item to auto-flag **LOW** during a count.
- **Central logging (optional):** set `SUBMIT_ENDPOINT` inside `inventory.html` to a Google Apps
  Script Web App URL to POST completed counts to a sheet. Left empty, counts export via
  Copy / CSV / JSON / Email / Print-PDF.

## Deploy on GitHub Pages
Upload the files, then Settings → Pages → Source: Deploy from a branch → `main` / root.
Site is `https://<user>.github.io/<repo>/`. Test locally first with `python3 -m http.server 8080`.

## Writing the NFC tags
| Tag on… | URL to write |
|---|---|
| Officer card | `https://<user>.github.io/<repo>/` |
| Unlabeled Kit (List 1) | `…/inventory.html?kit=list1` |
| Engineering | `…/inventory.html?kit=engineering` |
| BNQ First Aid | `…/inventory.html?kit=bnq` |
| Roof / Penthouse | `…/inventory.html?kit=roof` |
| Spa | `…/inventory.html?kit=spa` |
| Kitchen | `…/inventory.html?kit=kitchen` |
| Housekeeping (HSKP) | `…/inventory.html?kit=hskp` |
| Front Desk Bandaid Kit | `…/inventory.html?kit=frontdesk` |
| FCR Med Bag | `…/inventory.html?kit=fcr` |

Use **on-metal / ferrite-backed NTAG215/216** tags on the kits (they sit on metal). NTAG213 is
fine for the officer card. Write with the free **NFC Tools** app, then **lock** each tag.
