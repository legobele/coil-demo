# COIL — Unit 01 · by Nivelato Labs

Interactive embedded-screen UI for a vending machine. COIL is its own
product: it is made by Nivelato Labs (small byline on the attract and
thank-you screens), but it is **decoupled from Nivelato**, the field
measurement app — different name, different mark, different job. It
does not measure anything; it dispenses snacks.

Design language inherited from the Labs system (monochrome black/white
chrome, Anybody Expanded headlines, mono HUD labels, measurement-grid
backdrop, Liquid Glass on interactive controls only) with COIL's own
mark: a squared spiral — the vending coil, drawn end-on.

Touch-first scale pass: built for a standing user poking a physical
screen, one hand often holding a phone. Primary actions are 88px+
tall at the 1080-wide design scale, secondaries 72px+, product cards
are whole-card tap targets with 26px gaps.

## Run it

Open `index.html` in a browser, or serve the folder statically:

    python3 -m http.server 8321 --directory ~/workspace/vending-ui

Designed for a 1080x1920 portrait touchscreen; scales down to a
phone browser for demoing.

## Flow

Attract (COIL mark, clock + BROWSE QR) → catalog (9 products,
code-drawn pack art, stock HUD tags, one sold out) → product detail
(full nutrition panel, allergens, ingredients, quantity stepper,
full-width BUY bar) → QR pay screen (90-second payment window,
countdown ring) → dispensing animation → thank-you → back to
attract. Stock decrements after a vend.

## The two QRs

- **Browse QR** (`assets/qr-browse.png`) — persistent on the attract
  screen and in the catalog header. Encodes
  `https://nivelatolabs.com/coil/` — COIL's future product page
  under the Labs site. Demo status: that page does not exist yet
  either; the QR itself is real and decodes to that URL.
- **Pay QR** (`assets/qr-pay.png`) — checkout screen only, shows the
  transaction total. Encodes the demo payment link
  `https://nivelatolabs.com/coil/pay/UNIT01`. Design demo only:
  WeChat Pay / Alipay / phone wallets are text labels, not a real
  payment integration and not official logos.

Both PNGs were generated locally with Python `qrcode` and verified
decodable with OpenCV (re-verified after the COIL rebrand).

On the pay screen, the small "Demo only — simulate paid" link
completes the flow without a phone; if the 90-second window lapses
first, you return to the product with a "no charge" note. Cancel on
the pay screen also charges nothing.

No AI-generated images anywhere: all product visuals are SVG drawn
in code inside `index.html`. Font: Anybody Variable (OFL), copied
from the Nivelato UI draft.
