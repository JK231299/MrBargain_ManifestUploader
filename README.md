# Mr Bargain · Manifest → Hike

A browser tool that turns a Costco or Amazon manifest into a Hike-ready import file.
Everything runs in the staff member's browser — no server, no cost, nothing uploaded anywhere.

## Put it online (free, GitHub Pages)

1. Create a new GitHub repo (e.g. `mrb-hike-tool`). Public is fine; the tool holds no data.
2. Upload **`index.html`** to the repo (drag-and-drop on github.com works, or `git push`).
3. In the repo: **Settings → Pages**.
4. Under *Build and deployment*, set **Source: Deploy from a branch**, **Branch: `main`**, folder **`/ (root)`**, then **Save**.
5. Wait ~1 minute. Your link appears at the top of the Pages settings, like:
   `https://<your-username>.github.io/mrb-hike-tool/`
6. Share that link with staff. Bookmark it on the warehouse PC.

To update the tool later, edit `index.html`, commit/push, and Pages redeploys automatically.

## How staff use it

1. **Pick the source** — Costco or Amazon.
2. **Upload** the manifest `.xlsx`.
3. **Set options**:
   - Costco: SKU date suffix (e.g. `150826`).
   - Amazon: supplier, SKU source (ASIN / LPN / random), shipment, shipping cost per unit, suffix.
     The shipping cost fills in from supplier + shipment (Perth $0, Melb/Syd Container $1.50, Pallets $2.50) and can be overwritten for any load.
     The manifest must have a `RemovalReason` column (Overstock = 27% rate, everything else 24%); without it the tool stops with an error.
   - Product tags (optional): type your own, comma-separated, and choose whether they replace or add to the default tags. Leave blank for the defaults.
4. **Build Hike file**, check the preview and stats, then **Download**.
5. Import the downloaded file into Hike and map fields (map same-named columns; leave Stock, Handle and blank columns empty).

## Notes

- **First load** pulls the Python engine (~10 MB) from a CDN; give it a few seconds. After that it's cached.
- **Amazon barcodes** come from the manifest's UPC column (falls back to EAN, then a random code; the result shows how many fell back).
- **ASIN mode** groups duplicate items and sums quantity. **LPN mode** keeps every row — only use it when every LPN is filled.
- The 60-column Hike template is built into the tool, so staff never upload it.
- If the engine says it failed to load, it's almost always the network — refresh.

## Changing the rules

All the pricing/tag/SKU logic lives in one `<script id="pycode">` block inside `index.html`.
It mirrors the Colab notebooks exactly, so any formula change there can be copied here.
