# Izzies Coffee Shops

A single-page coffee shop scoreboard: top three, full sortable list, map, and an add/edit form — all in one HTML file, with the ratings kept in `data.json` in this repo so every device stays in sync.

## Files

| File | What it is |
|---|---|
| `index.html` | The whole app — including the background and header photos, embedded, so there are no image files to upload and it still looks right offline. |
| `data.json` | The 13 shops from the original spreadsheet. The page reads and writes this file. |

`index.html` also has a copy of the data baked in, so it still works if you open it before setting up sync.

## 1. Put it on GitHub

1. On github.com, **New repository** → name it something like `izzies-coffee` → **Public** → Create.
2. **Add file → Upload files**, drag in `index.html` and `data.json`, commit.
3. **Settings → Pages** → Source: *Deploy from a branch*, Branch: `main`, folder `/ (root)` → Save.
4. After a minute your page is live at `https://<your-username>.github.io/izzies-coffee/`.

## 2. Turn on syncing

The page saves changes straight back into `data.json` using a GitHub token.

1. github.com → your avatar → **Settings** → **Developer settings** → **Personal access tokens** → **Fine-grained tokens** → **Generate new token**.
2. Name it "Izzies Coffee", set an expiry, **Repository access → Only select repositories** → pick `izzies-coffee`.
3. Under **Repository permissions**, set **Contents** to **Read and write**. Nothing else is needed.
4. Generate, and copy the token (it is shown once).
5. Open your page, tap **⚙︎ Sync**, fill in your username, repo name, branch `main`, path `data.json`, paste the token, **Save settings**.

Do that once on the iPad and once on the phone and both write to the same file. The pill in the header shows **Synced** / **Unsaved** / **Saving…**. Saves happen automatically a second or two after any change, and the page re-reads the file whenever you come back to the tab, so a shop added on the phone shows up on the iPad.

### If you'd rather the ratings weren't public

A public repo means anyone who finds the URL can read `data.json`. GitHub Pages on a **private** repo needs a paid plan, so the neat trick is: keep `index.html` in the public repo (that's what Pages serves) and put `data.json` in a **separate private repo**. In ⚙︎ Sync just point the repo/path fields at the private one and give the token access to it. The page works the same.

## 3. Add it to the home screen

- **iPad / iPhone**: open the page in Safari → Share → **Add to Home Screen**. It opens full-screen like an app.
- **Android**: Chrome menu → **Add to Home screen**.

## Optional: live Google ratings and reviews

Each shop's card can show its current Google rating, review count and the three most recent reviews. That needs your own API key:

1. [console.cloud.google.com](https://console.cloud.google.com) → create a project → **APIs & Services → Library** → enable **Places API (New)**.
2. **Credentials → Create credentials → API key.**
3. **Restrict** it: under *Application restrictions* choose **Websites** and add your Pages URL (`https://<your-username>.github.io/*`); under *API restrictions* limit it to Places API (New).
4. Paste it into **⚙︎ Sync → Google Places API key**.

It's stored only in that browser and is never written to `data.json`, so it doesn't end up in the repo. Google's free monthly allowance covers this kind of use comfortably, but the key does need billing enabled on the project. Ratings refresh automatically when they're more than 30 days old (Google's terms don't allow caching their content longer than that), and review text is deliberately not saved to the repo.

Without a key nothing breaks — the card just shows the search links instead.

## Food hygiene ratings

Every card has a **Check** button that queries the Food Standards Agency's free API and saves the result. Eleven of the thirteen originals are already filled in (checked 1 Sep 2026):

- **5 — Very good**: Coffee#1, Fino Lounge, Against The Grain, Blossom, Bentleys, Tŷ Melin, A-Head of the Game
- **4 — Good**: Asda Coryton (the cafe is registered to Eurest), The Wellfield Social
- **3 — Generally satisfactory**: Pink Kiwi
- **Awaiting inspection**: The Coffi House (re-registered recently, so any older rating you see elsewhere is stale)
- **No record**: Ground Bakery Whitchurch, Coffee Club Whitchurch

One caveat: I could not reach the FSA API from the machine I built this on, so the live **Check** button is written but untested. If it doesn't work from your browser it says so and gives you a link to ratings.food.gov.uk instead — the pre-filled ratings above are unaffected.

## Using it

- **Top three** sits at the top and re-ranks itself as scores change.
- **Sort** by overall score, name, date added, or any single category — and pick **My own order** to drag rows (desktop) or nudge them with ↑ ↓ (touch). That hand-made order is saved with the data.
- **Search** filters on name, address and notes.
- **Tap a shop** (anywhere on the row, or the ⓘ button, or *Full card* in a map pin's popup) to open its card: your nine grades with bars, your notes, its food hygiene rating, Google reviews, and search links for TripAdvisor, Google Maps and Facebook.
- **+ Add a coffee shop** sits above the map as well as in the header: name, address, the nine grades, notes. The overall score is the average of whichever grades you fill in — you can leave some blank.
- **Pins**: type an address and hit *Find on map*, use *Use my location* while you're sat in the place, or just tap the map. Drag the pin to fine-tune. Twelve of the thirteen originals are pre-pinned; *Coffee Club Whitchurch* couldn't be identified, so it needs a pin the next time you're there.
- **Backups**: ⚙︎ Sync → Export data.json / Export CSV, and Import to restore.

## Notes

- Scores are the mean of the nine categories: Atmosphere, Price, Taste, Cakes, Idiots, Food, Parking, Views, Friendliness of Staff.
- Map tiles and address search come from OpenStreetMap. Leaflet loads from a CDN, so the map needs a connection — offline, the list and the add form still work.
- Everything is stored in this repo and in your browser. There is no server and no account.
