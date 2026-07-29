# CLAUDE.md

Guidance for Claude Code working in `~/Projects/many-happy-returns/`.

A forty-two year artwork: one birthday song stretched from 2026 to 2068, served
as two static files. See `README.md` first.

## The rule that outranks everything

**`EPOCH` can never change.** It is not a setting, it is the piece. It is
midnight on 29 July 2026 in Asia/Jerusalem, stored as `2026-07-28T21:00:00Z`.
Changing it moves the whole work under everyone who has ever visited. If a task
seems to require changing it, stop and ask.

`TOTAL_YEARS` is nearly as fixed — it sets the ratio, so altering it moves every
future moment of the piece.

Safe to change forever: layout, colour, copy, the field, the panel. Redesign it
in 2040 and the piece does not flinch, because position is derived from the
clock and nothing else.

## Character-only

Every visible form is a typed glyph. The field is `fillText`; the panel is
monospace text; the progress bar is `▓░`. No images, SVG, `arc`/`rect`-as-art,
or CSS shapes carrying the design. CSS may do layout, colour and a legibility
text-shadow. This is the house rule across the user's character work — see the
`character-only-art` skill in `~/Projects/characterworld/skills/`.

Density matters as much as the rule: dense full-field, accumulation and decay,
zalgo prominently visible and mapped to a real quantity (here, elapsed time).
Sparse diagram-like layouts get rejected.

## Verify with the clock, not by waiting

`?at=21y`, `?at=50%`, `?at=6months`, `?at=90` move the audio as well as the
readout. Always check a far-future position after touching the field or the
timing — corrosion and position bugs are invisible at t≈0.

Serve over http; `fetch()` refuses `file://`.

```sh
python3 -m http.server 8000
```

## Things that will bite you

- **Never vary `WINDOW_SIZE` or `RENDER_RATE` per device.** The frame index is
  `elapsed * sampleRate / hop`, and the phases are seeded from it. Making either
  depend on the visitor's hardware means two listeners at the same instant hear
  different noise. `RENDER_RATE` is pinned at 44100 precisely for this.
- **Dates must render in local time, not `toISOString()`.** The epoch is 21:00Z,
  so UTC formatting prints the day *before* the birthday. There is a `localDate`
  helper; use it for anything calendar-facing. The clock line is UTC and is
  labelled `Z`.
- **The forward FFT only runs when `floor(position)` moves** — about four times
  a day at this ratio. That is exact, not an approximation, and it is what makes
  a 65536-point window affordable on a phone. Do not "fix" it into running every
  frame.
- **The window is Paul's `sqrt(1 − |x|^2.5)`, not a Hann.** Random phases make
  frames add in power, so `sum(w²)` must stay flat across the 50% overlap. A
  Hann there tremolos audibly.
- Silencing must stop the buffers already scheduled ahead, not merely stop
  queueing new ones.

## Credit

The algorithm is Nasca Octavian Paul's (*Paul's Extreme Sound Stretch*); the
song is Monk Turner + Fascinoma's. Both credits are in `README.md` and the song
credit is on the page itself. Do not drop either while refactoring.

Related: the JUCE implementation of the same algorithm lives at
`~/Projects/extremestretch/`.
