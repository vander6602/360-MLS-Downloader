<p align="center">
  <img src="logo.svg" alt="360 MLS Downloader" width="200">
</p>

<h1 align="center">360 MLS Downloader</h1>

<p align="center">Download 360-degree panoramic images and listing data from MLS virtual home tours.<br>Auto-detects platform from URL — just paste and go.</p>

<p align="center"><strong>v2.0.0</strong></p>

Built by **PRHack | CyberSpartan77** ([@fjimenez77](https://github.com/fjimenez77))

---

## Supported Platforms

| Platform | What You Get |
|----------|-------------|
| **Zillow 3D Home** | 360° panoramas (4K JPEG + 8K AVIF), all listing photos, property details (price, beds, baths, sqft, lot, year built), full description |
| **Zillow (no 3D)** | All listing photos, property details, description — works on any Zillow listing |
| **Ricoh360 MLS** | 360° panoramas (original + AI-enhanced), tour metadata, photographer info |

## Features

- Auto-detects platform from URL — just paste and go
- Download all 360° equirectangular panoramas from MLS virtual tours
- **Zillow:** Downloads listing photos, property description, price, beds/baths/sqft, year built, lot size, MLS #
- **Zillow:** Works with or without a 3D tour — grabs everything available on the listing
- **Zillow:** Opens a browser for CAPTCHA solving if needed (user solves, app continues automatically)
- Interactive menu mode with tour analysis, room selection, and size estimation
- CLI mode with flags for scripting and automation
- Downloads both original and AI-enhanced/8K versions
- Selective room downloads (pick specific rooms or ranges)
- Generate a self-contained 360° HTML tour viewer for offline use
- Resume support — skips already-downloaded files
- Saves full tour metadata as JSON
- Includes usage instructions for uploading to real estate platforms
- Cross-platform: Mac and Windows support

## Requirements

- Python 3.8+
- `requests` library
- `playwright` (required for Zillow — auto-installs on first use)

## Installation

```bash
git clone https://github.com/fjimenez77/360-MLS-Downloader.git
cd 360-MLS-Downloader
pip install requests
```

For Zillow support, Playwright will auto-install on first use. Or install manually:

```bash
pip install playwright
python3 -m playwright install chromium
```

---

## Quick Start

### Interactive Menu

```bash
python3 mls360-menu.py
```

This launches a full interactive menu where you can:

1. Paste any tour URL (Zillow or Ricoh360 — auto-detected)
2. View tour details (address, photographer, price, room count)
3. Browse all rooms with enhancement status
4. Choose what to download
5. View direct image URLs
6. Estimate total download size before committing
7. Build a 360° HTML viewer for offline use

You can also pre-load a tour URL:

```bash
python3 mls360-menu.py "https://www.zillow.com/homedetails/ADDRESS/ZPID_zpid/"
python3 mls360-menu.py "https://mls.ricoh360.com/TOUR-ID/ROOM-ID"
```

### CLI Mode (Advanced)

```bash
# Download a Zillow listing (3D tour + photos + details)
python3 mls360-downloader.py "https://www.zillow.com/homedetails/ADDRESS/ZPID_zpid/"

# Download a Ricoh360 tour
python3 mls360-downloader.py "https://mls.ricoh360.com/TOUR-ID/ROOM-ID"

# Just the tour UUID works too (Ricoh360)
python3 mls360-downloader.py f948586f-1c5c-48dc-81fd-6ef9a09a12c0

# Custom output directory
python3 mls360-downloader.py TOUR-URL --output ~/Desktop/my-listing

# Only AI-enhanced/8K images
python3 mls360-downloader.py TOUR-URL --enhanced-only

# Only originals/4K (skip enhanced)
python3 mls360-downloader.py TOUR-URL --originals-only

# Just grab metadata JSON, no images
python3 mls360-downloader.py TOUR-URL --json-only
```

---

## Usage Guide

### Step 1: Get the URL

**For Zillow** — copy the listing URL from your browser:
```
https://www.zillow.com/homedetails/9123-Pitcairn-San-Antonio-TX-78254/26433581_zpid/
```

**For Ricoh360** — copy the tour URL:
```
https://mls.ricoh360.com/f948586f-1c5c-48dc-81fd-6ef9a09a12c0/c84e8d06-2b82-46a0-991a-8814573e048b
```

### Step 2: Run the Downloader

```bash
python3 mls360-menu.py
```

Select option `1`, paste the URL. The tool auto-detects the platform and:
- Fetches tour/listing data
- Shows details (address, rooms, price, photos)
- Lets you choose what to download

For Zillow: a browser window opens briefly to load the page. If a CAPTCHA appears, solve it — the app continues automatically once the page loads.

### Step 3: Use Your Images

**Zillow download output:**

```
~/Downloads/mls360-Address/
  HOW TO USE THESE IMAGES.txt   # Usage instructions
  listing-details.txt           # Price, beds, baths, sqft, description
  tour-data.json                # Full tour metadata
  photos/                       # All listing photos
    01-listing-photo-1.jpg
    02-listing-photo-2.webp
    ...
  rooms/                        # 360° panoramas (if 3D tour exists)
    01-Front-yard/
      original.jpg              # 4K panorama (JPEG)
      preview.jpg               # Thumbnail
      enhanced.avif             # 8K panorama (AVIF, when available)
    02-Entrance/
      ...
```

**Ricoh360 download output:**

```
~/Downloads/mls360-Tour-Name/
  HOW TO USE THESE IMAGES.txt   # Usage instructions
  tour-data.json                # Full tour metadata
  brand-logo.jpg                # Photographer's brand logo
  tripod-cover.jpg              # Tripod cover overlay
  rooms/
    01-Foyer/
      original.jpg              # Original 360 panorama
      preview.jpg               # Smaller preview
      enhanced.jpg              # AI-enhanced version (if available)
      enhanced-preview.jpg      # Enhanced preview
    02-Kitchen/
      ...
```

**Image format:** JPEG equirectangular projection — standard 360 format supported by:
- MLS platforms (Zillow, Realtor.com, Redfin)
- 360 tour builders (Kuula, CloudPano, Matterport)
- Social media (Facebook 360 photos, YouTube 360)
- VR headsets
- Any panorama viewer

### Step 4 (Optional): Build a 360° HTML Viewer

Use menu option `7` to generate a self-contained HTML tour viewer from any downloaded tour:

1. Select option `7` from the main menu
2. Pick a downloaded tour folder
3. The tool generates `tour-viewer.html` in that folder
4. Launch it using the included scripts:
   - **Mac:** Double-click `Open Tour Viewer.command`
   - **Windows:** Double-click `Open Tour Viewer.bat`

The viewer includes a sidebar with room thumbnails, previous/next navigation, keyboard controls (arrow keys), and works completely offline.

---

## Uploading to Real Estate Platforms

The downloaded images are standard equirectangular JPEGs — most platforms auto-detect them as 360° photos:

| Platform | How to Upload |
|----------|---------------|
| **Zillow (FSBO)** | Upload room images as regular photos — Zillow auto-detects 360° format |
| **Realtor.com** | Upload through your listing dashboard |
| **Redfin** | Upload through the listing photo manager |
| **MLS** | Upload via your agent or FSBO MLS service |
| **Facebook** | Upload as a "360 Photo" — auto-detected |
| **Kuula / CloudPano** | Upload equirectangular JPEGs to create interactive tours |

**Tips:**
- Use `enhanced` versions when available (better lighting and color)
- Upload rooms in order (01, 02, 03...) to keep the tour flow logical
- Each image is typically under 2 MB, within most platform upload limits

A detailed instructions file (`HOW TO USE THESE IMAGES.txt`) is included in every download.

---

## Interactive Menu Options

| Option | Description |
|--------|-------------|
| **1. Set target URL** | Enter a tour URL (Zillow or Ricoh360 — auto-detected) |
| **2. View tour info** | Show address, photographer, price, room count, listing details |
| **3. View all rooms** | Table of all rooms with original/enhanced status |
| **4. Download images** | Submenu: all, all + JSON, enhanced-only, originals-only, selective, JSON-only |
| **5. View direct URLs** | Show/save all direct image URLs |
| **6. Estimate size** | Check total download size before downloading |
| **7. Build 360° viewer** | Generate an offline HTML tour viewer from a downloaded tour |
| **q. Quit** | Exit the application |

### Selective Downloads

In the download menu, option `5` lets you pick specific rooms:

```
Enter room numbers: 1,3,5,10      # Individual rooms
Enter room numbers: 1-5            # Range
Enter room numbers: 1-5,10,15-20   # Mixed
Enter room numbers: all            # Everything
```

---

## CLI Flags

| Flag | Description |
|------|-------------|
| `--output`, `-o` | Set custom output directory (default: `~/Downloads/mls360-<name>`) |
| `--enhanced-only` | Only download enhanced/8K images |
| `--originals-only` | Only download original/4K images |
| `--json-only` | Save tour metadata JSON without downloading images |
| `--version` | Show version number |

---

## File Structure

```
360-MLS-Downloader/
  mls360-menu.py              # Interactive menu interface
  mls360-downloader.py        # CLI interface
  mls360_downloader_core.py   # Shared core engine (provider-agnostic)
  mls360_viewer.py            # 360° HTML viewer generator
  providers/
    __init__.py               # Provider registry and auto-detection
    zillow.py                 # Zillow 3D Home + listing provider
    ricoh360.py               # Ricoh360 MLS provider
  vendor/
    pannellum.min.js          # Pannellum 2.5.6 (MIT) — 360° viewer
    pannellum.min.css
  logo.svg
  README.md
  LICENSE
  CONTRIBUTING.md
```

---

## How It Works

The tool uses a multi-provider architecture to support different platforms:

**Zillow:**
1. Opens a browser to load the listing page (bypasses bot protection)
2. Extracts property details, photo URLs, and 3D tour IDs from the page
3. Fetches the IMX manifest from Zillow's CDN (public, no auth)
4. Downloads 4K/8K panoramas, listing photos, and saves property details

**Ricoh360:**
1. Extracts the tour UUID from the URL
2. Fetches the Next.js build ID from the main page
3. Calls the Next.js data endpoint for full tour metadata
4. Constructs direct S3 URLs and downloads all images

Both providers output to the same folder structure, and the 360° HTML viewer works with either.

---

## Credits

**Author:** PRHack | CyberSpartan77 ([@fjimenez77](https://github.com/fjimenez77))

API reverse-engineering powered by [AuthScope](https://github.com/fjimenez77/AuthScope) Chrome Extension and manual network analysis.

360° viewer powered by [Pannellum](https://pannellum.org/) (MIT License).

---

## Contributing

Want to help improve this tool? Contributions are welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on how to fork, branch, and submit a pull request.

---

## Disclaimer

This tool is intended for downloading your own property tour images or tours you have permission to use. Respect photographers' copyright and licensing agreements. The images may be owned by the photographer or real estate agency — verify your usage rights before republishing.

---

## License

MIT License — see [LICENSE](LICENSE) file.
