# CoinWise Dashboard

Web dashboard for **CoinWise**, a smart coin bank that sorts HK$1 / $2 / $5 coins by weight
(Arduino UNO + HX711 load cell + two SG90 servos).
Group project for INT4089 "Technologies in STEM and AI Education", EdUHK, 2026/27 Sem 1.

Live demo: https://louisli185.github.io/coinwise-dashboard/

## Features

- **Count Coins** — counts and totals per denomination, value-distribution bars, savings goal with progress (tie it to a real wish — bubble tea, cinema ticket…), per-day history (saved in the browser)
- **Maths Challenge** — "Can you make HK$14?" game whose numbers are drawn from your own bank total — the more you sort, the bigger the challenges
- **AI Money Coach** — offline local-maths answers (always works) + optional DeepSeek via Poe backend with automatic fallback; one-step challenge hints (never the answer) and kid-friendly explanations of the science inside the machine
- **Coin sounds** — synthesized coin "clinks" with a different pitch per denomination, no audio files
- **Real (Arduino) mode** — Web Serial connection, machine states (READY / WEIGHING / SORTING / ERROR), live weight readout, 60-point sparkline, and a weight-band axis showing the recognised ranges
- **Simulator** — test the whole serial pipeline without hardware ("Arduino messages" panel at the bottom)

## Running

- **Online**: open the live demo link (Chrome or Edge recommended)
- **Offline**: open `index.html` directly in a browser — everything except the AI backend works without network
- **With the Arduino**: switch Coins to "Real (Arduino)" and press Connect. Web Serial needs Chrome/Edge on a desktop computer; pick the baud rate that matches the firmware.

## Serial protocol (firmware reference)

One message per line, newline-terminated. `parseArduinoLine` in `index.html` accepts:

| Line | Meaning |
| --- | --- |
| `{"type":"coin","value":2}` | a coin was identified |
| `{"type":"weight","value":8.41}` | a weight reading in grams (`grams` / `g` keys also accepted) |
| `{"type":"status","value":"READY"}` | machine state: READY / WEIGHING / SORTING / ERROR |
| `2` | bare integer in [1, 2, 5] = coin |
| `8.41` | any other bare number = weight |
| `READY` / `ERROR` / ... | bare status word (REJECT maps to ERROR) |

If the firmware never sends status lines, the page auto-advances WEIGHING → SORTING → READY on its own.

## Files

- `index.html` — current dashboard (v3)
- `INT4089-v2.html` — earlier working draft, kept for reference

## Updating the site

Copy the new dashboard to `index.html`, commit and push to `main` — GitHub Pages rebuilds automatically.
