# many happy returns

One birthday song stretched across forty-two years.

**https://willbearfruits.github.io/many-happy-returns/**

It began at midnight on 29 July 2026 and ends on 28 July 2068. Nothing is stored
and nothing is streamed: the page works out where the piece has got to from the
wall clock and synthesises that moment in the browser. Arrive in 2047 and you
hear the middle of the song, because the middle of the song is where 2047 is —
not because anything has been playing in the meantime.

The title is the traditional birthday greeting and also a description of how it
works. You return; it has gone on without you.

```
elapsed        = now − epoch
sourcePosition = elapsed / 36,714,000
```

## Credit

The stretching algorithm is **Nasca Octavian Paul's** — *Paul's Extreme Sound
Stretch*. Throw the phases away, replace them with noise, overlap enormous
windows. The whole piece rests on his idea; this is an independent
implementation written from the method, not a port of his code.

The song is **“It’s Your Birthday!” by Monk Turner + Fascinoma**, released
**[CC BY 3.0](https://creativecommons.org/licenses/by/3.0/)** — verified against
the file's own Vorbis `copyright` tag and against the
[Free Music Archive listing](https://freemusicarchive.org/music/Monk_Turner__Fascinoma/The_New_Birthday_Song_Contest/Its_Your_Birthday_1839/).

CC BY 3.0 permits commercial use and adaptations. What it requires is
attribution: the title, the artist, a link to the source, a link to the licence,
and — because stretching it 36.7 million times is unmistakably an adaptation —
an indication that the work was modified. All five are on the page itself. **Do
not trim that credit down**; it is the licence condition, not decoration.

There is a pleasing symmetry in the source: it won WFMU and the Free Music
Archive's contest to write a *copyright-free replacement* for "Happy Birthday to
You", which was locked up at the time. The one birthday song anyone was free to
use is the one that gets to run for forty-two years.

## Two files

`index.html` and `source.ogg`. No build, no dependencies, no server. Any static
host will do — it publishes itself from `main` via GitHub Pages.

```sh
python3 -m http.server 8000   # must be http; fetch() refuses file://
```

## Configure

At the top of the `<script>` block:

| constant | meaning |
| --- | --- |
| `EPOCH` | **the one value that can never change.** It is the piece. Midnight 29 July 2026 in Asia/Jerusalem, stored as the true UTC instant `2026-07-28T21:00:00Z` |
| `TOTAL_YEARS` | 42 |
| `WINDOW_SIZE` | FFT size, power of two. Longer is glassier. Changing it does not move the piece, but it does change which noise you hear at a given moment |
| `RENDER_RATE` | pinned at 44100 for everyone, so listeners on 48 kHz hardware render identical audio |

Everything else — layout, colour, copy — is safe to change forever. Redesign it
in 2040 and the piece does not so much as flinch.

## Previewing the future

You cannot wait twenty-one years to check it works:

```
?at=21y      21 years in — the middle
?at=50%      same thing
?at=6months
?at=90       90 seconds in
```

This moves the audio as well as the readout.

## How it stays in sync

Every listener derives the frame index from the clock, and the random phases are
seeded from that index — so two people whose clocks agree to within one frame
(~0.74 s) render bit-identical audio. It is a broadcast, not a copy each person
happens to hold. The page also reads the `Date` header off its own request and
trusts that over the visitor's clock.

## The look

Character-only: every visible form is a typed glyph. The field is `fillText`
over a density ramp `·˙:░▒▓█` with CP437 and maths glyphs through the middle
tiers; the panel is monospace text and the progress bar is `▓▓▓░░░`. No images,
no SVG, no shapes carrying the design.

The field shows **the spectrum of the frame currently sounding**, mirrored about
the vertical axis with the lowest frequencies at the centre, so it builds a
symmetrical mass rather than scanning left to right. A darker elliptical niche is
carved out of the density for the inscription, and the glow gathers around it.

**Zalgo is mapped to elapsed time.** Combining marks accumulate on hot cells as
the piece ages — one mark in 2026, a thicket of five or six by 2068. The field
corrodes across the forty-two years.

## Sound

Starts on arrival. Browsers block autoplay until a visitor has interacted with
the site once, so a first-ever visit stays silent until any tap or keypress;
every return sounds immediately. `[ listen ]` / `[ silence ]` toggles it —
silencing stops the buffers already scheduled ahead, and starting again
re-derives the position from the clock.

At 36.7 million ×, the read head advances **one sample of source roughly every
14 minutes**. A day of listening moves you 2.3 ms into the song; a year moves
you 0.86 seconds. Recorded as a 24-bit stereo file the whole span would weigh
around 351 TB, which is the other reason it is synthesised rather than served.

## Prior art

Jem Finer's *Longplayer* has run since 1 Jan 2000 on a 1000-year cycle and
computes its position from the clock in the same way. John Cage's *Organ²/ASLSP*
in Halberstadt is 639 years long and treats chord changes as events you travel
to attend.
