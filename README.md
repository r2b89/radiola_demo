# Old Radio

A small demo for [MFD Studio](#what-is-mfd-studio). It's an old FM radio you can actually listen to:
turn the knobs, tune between stations, hear the static in between.

The radio face isn't HTML or CSS. It was drawn in MFD Studio and is shown here by the MFD player.
The same display file can also run as native code (C + NanoVG) on desktops and devices, so the
radio looks and works the same on the web, on a phone and in a native app. That's the point of
this demo.


## What it does

- Finds real internet radio stations near you (within ~100 km) through
  [Radio Browser](https://www.radio-browser.info/). If there aren't many, it adds stations from
  your country.
- If you don't share your location it falls back to Berlin. You can type any city in the box
  under the radio ([Open-Meteo](https://open-meteo.com/) geocoding does the lookup). The city is
  remembered for next time.
- Stations get a place on the FM scale (87.5–108 MHz). If the station's name mentions its
  frequency, like "Radio 104.6", it goes there; the rest are spread across the dial.
- Tune off a station and you hear noise, close to it and it fades in. The signal meter and the
  stereo light follow along.
- The five most popular stations go on the preset buttons.
- Five looks, one per era: 1930, 1950, 1970, 1980 and 2000. Switching keeps the tuning, volume
  and power as they were.

Touch a knob to get its − / + buttons. Hold them to turn faster.

Only MP3 and AAC streams are used. On an https page, http streams are skipped too, since
browsers block them. A stream that fails to play is marked and skipped.

## URL options

| Parameter       | What it does                                                         |
|-----------------|----------------------------------------------------------------------|
| `skin`          | Start with this year's look (`1930`, `1950`, `1970`, `1980`, `2000`) |
| `city`          | Look for stations around this city                                   |
| `lat` / `lon`   | Or give exact coordinates                                            |


For example: `index.html?skin=1970&city=Tallinn`

With `token` the year buttons are hidden and you get whatever display was published under that
token. That's handy for trying your own radio design from the editor.

## How it's put together

`index.html` holds three things:

1. the MFD player (`<mfd-display>` web component),
2. the five radio displays, packed,
3. a bit of JavaScript for stations and sound.

The display and the page talk only through the display's parameters:

- the page reads `tune`, `power` and `volume`, which the knobs change;
- the page writes `station`, `signal`, `stereo` and the preset labels `p1`…`p5`;
- a preset button sends a `preset` event with its number.

All the drawing, knobs and animation are in the display. All the networking and audio are in the
page. So you can redesign the radio in MFD Studio, keep the parameter names, and the page doesn't
need to change.

If the displays aren't inlined, the page loads `radio-<year>.mfd.json` next to it, and the player
from the backend (or `../../mfd-player.js`).

## What is MFD Studio

MFD Studio is an editor for building UI like gauges, dashboards and instrument panels once and
running it everywhere. You draw the display and wire it up with formulas, then export it:

- for the web and mobile browsers, as an embed or a single HTML file with the JavaScript player;
- for native apps and devices, as generated C code drawn with NanoVG (Metal on macOS, OpenGL
  elsewhere, or a software renderer for hardware without a GPU).

Formulas behave the same in every player, so what you see in the editor is what you get on each
platform.

## Credits

Station list from [Radio Browser](https://www.radio-browser.info/), a free community project.
Please be nice to their servers. City search by [Open-Meteo](https://open-meteo.com/). The
streams belong to their stations.

## Contacts

Questions or ideas: thorbotson at gmail dot com.
