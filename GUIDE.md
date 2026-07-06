# Auktionskatalog – Plan & Deployment Guide

A free, single-file website that helps the auction group build their catalogue list (Katalog nr. · Varebeskrivelse · Butikspris) and download it as Excel (.xlsx) or CSV — ready to feed into their existing document-generating solution. Bids are not entered here (customers write those on the printed catalogue), but the exported file still includes an empty "Bud" column so it matches the expected input format. The site also has a short built-in "Sådan bruges siden" description with an example at the top.

---

## 1. What the site does

- **Fast entry flow:** type a description, press **Enter** → the row is saved and the catalogue number automatically increments to the next one.
- **Speech-to-text (Danish):** click the 🎤 button and dictate the description. Uses the browser's built-in Web Speech API set to `da-DK`. Works in **Chrome and Edge** (desktop and Android). Not supported in Firefox; on iPhone, use the keyboard's own dictation button instead.
- **Hands-free entry:** while dictating, saying **"næste"** or **"enter"** saves the row and jumps to the next catalogue number, so a whole list can be dictated without touching the keyboard (e.g. "HP laserprinter … næste … Gastronoma airfryer … næste"). The command is only acted on once the browser has finalized the phrase, so it won't trigger on half-recognized words. Note: this means the words "næste"/"enter" can't be *dictated* into a description — type them manually or edit the row afterwards if needed.
- **Editable table:** click any cell to fix typos, delete rows with ✕, and use "Omnummerér" to re-sequence numbers after deletions.
- **Export:** "Download Excel (.xlsx)" produces a real Excel file (via the SheetJS library, loaded from a CDN). "Download CSV" produces a semicolon-separated, UTF-8 (with BOM) file so Danish Excel opens æ/ø/å correctly.
- **Autosave & crash safety:** the list is saved automatically in the browser (localStorage) on every change. If the tab is closed, the browser crashes, or the internet drops, the list is restored on the next visit — on the same device and browser. A status line shows when it was last saved.
- **Offline:** once the page is open, adding rows, editing, autosave, and **CSV download** all work without internet. Only the Excel (.xlsx) button needs the page to have been *opened* with internet (its library loads from a CDN); if it wasn't, the site tells the user to use CSV instead. Speech-to-text also requires internet in most browsers.
- **Privacy:** everything stays in the browser on the user's own device; nothing is sent to any server.

## 2. Hosting plan (free, GitHub Pages)

GitHub Pages is the right choice here: free, no server needed (the site is one static HTML file), free HTTPS, and custom domain support.

### Step-by-step

1. **Create a GitHub account** at https://github.com (free).
2. **Create a new repository:** click "+" → "New repository". Name it e.g. `auktionskatalog`. Set it to **Public** (required for free Pages). Click "Create repository".
3. **Upload the site:** on the repo page, click "uploading an existing file" (or Add file → Upload files), drag in `index.html`, and click "Commit changes". The file **must** be named `index.html`.
4. **Enable GitHub Pages:** go to **Settings → Pages**. Under "Build and deployment", set Source = **Deploy from a branch**, Branch = **main** / root folder. Save.
5. Wait 1–2 minutes. The site is now live at:
   `https://<your-username>.github.io/auktionskatalog/`

To update the site later, just upload a new `index.html` over the old one (Add file → Upload files → commit). Changes go live within a minute or two.

**Tip:** if you name the repository `<your-username>.github.io` instead, the site lives directly at `https://<your-username>.github.io/` with no subpath.

## 3. Custom domain (paid part)

Only the domain itself costs money — hosting stays free.

### 3.1 Buy a domain

Any registrar works. Common options for a Danish group:

| Registrar | Notes | Typical price for .dk |
|---|---|---|
| Punktum dk-accredited registrars (e.g. DanDomain, one.com, Simply.com) | Needed for **.dk** domains | ~50–150 kr./year |
| Cloudflare Registrar | At-cost pricing, easy DNS, but **no .dk** | ~$10/year for .com |
| Namecheap / Porkbun | Cheap .com/.net/.site | ~$10–12/year |

Example: `auktionskatalog.dk` or `katalog.jeresgruppe.dk`.

### 3.2 Point the domain at GitHub Pages (DNS)

In your registrar's DNS settings:

**If using a subdomain like `www.example.dk` or `katalog.example.dk` (recommended):**

```
Type:  CNAME
Name:  www          (or "katalog")
Value: <your-username>.github.io
```

**If using the bare/apex domain `example.dk`, add four A records:**

```
Type: A   Name: @   Value: 185.199.108.153
Type: A   Name: @   Value: 185.199.109.153
Type: A   Name: @   Value: 185.199.110.153
Type: A   Name: @   Value: 185.199.111.153
```

(Optionally add AAAA records for IPv6: 2606:50c0:8000::153, :8001::153, :8002::153, :8003::153.)

### 3.3 Tell GitHub about the domain

1. In the repository: **Settings → Pages → Custom domain** → enter e.g. `www.example.dk` → Save. (This creates a `CNAME` file in the repo — leave it there.)
2. Wait for the DNS check to pass (can take from minutes up to 24–48 hours, usually fast).
3. Tick **"Enforce HTTPS"** once it becomes available — GitHub issues a free certificate automatically.
4. Verify: visit your domain, and check that `https://` works.

**Recommended:** in Settings → Pages, also verify your domain under your GitHub account settings (Settings → Pages → Verified domains) to prevent domain takeover if you ever delete the repo.

## 4. Possible later enhancements

- **Import an existing CSV/Excel** to continue a list.
- **Print-friendly view** matching the group's catalogue document layout.
- **Full PWA/offline support** so even opening the site works without internet (a small service worker file plus bundling the Excel library instead of loading it from the CDN). Still free on GitHub Pages.

## 5. Costs summary

| Item | Cost |
|---|---|
| Hosting (GitHub Pages) | 0 kr. |
| HTTPS certificate | 0 kr. (automatic) |
| Domain name | ~50–150 kr./year (.dk) |
| Total | Domain only |
