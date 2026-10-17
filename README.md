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

## The 12-inch rule (type floors)

The screen is read by a standing user from about 12 inches (30 cm).
Functional type never goes below these floors at the 1080 design
scale (1rem = 20px), no matter the layout:

| Element | Floor |
|---|---|
| Product names (cards + detail) | 44px |
| Prices (cards, detail, always bold) | 44px |
| Totals (detail total, pay total) | 44px / 60px |
| Card blurbs + all body copy | 30px |
| Nutrition values | 34px |
| Nutrition labels, stock tags, [ SOLD OUT ] | 24px |
| Pay instructions | 32px |
| Pay countdown numeral | 48px |

Decorative HUD chrome (corner tags, footer, slot codes in topbars)
may stay smaller; anything that informs a decision may not. The
catalog runs 2 columns so the type fits — fewer, bigger cards,
scrolling is fine. Below 700px wide (the phone demo), the root size
is held at the design scale so the floors stay absolute: the type
does NOT shrink, the layout reflows into a single column and the
screens scroll internally instead.

## Motion

One easing (`cubic-bezier(.22,1,.36,1)`) and three durations for the
whole product: `--dur-micro` 160ms (presses, hovers), `--dur-screen`
280ms (screen entrances, card stagger), `--dur-overlay` 240ms (QR
overlay). Screen changes run through the state machine: the stage
dips (fade + slight settle, 120ms) while screens swap, and the
incoming screen rises in. Buttons dip scale/brightness on press;
quantity ticks and the total pulses on change; the pay countdown
pulses under 10 seconds; a sold-out card answers a tap with one
small shake instead of navigating. `prefers-reduced-motion`
collapses all of it to instant state changes.

The vend sequence is staged on an outlined schematic of the machine
(slot window + coil shelf, dashed drop shaft, collection tray): the
purchased item tips off its shelf, falls with gravity easing while
the camera pans down tracking it, and lands resting fully inside
the tray outline — that final frame holds ~1.1s before Thank You.
Cans/bottles tumble 16°, bars 10°, packs 6°. Storyboard keyframes
live in `samples/vend-story-*.png`.

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
