# AlphaTech Traders — Session Handoff

**Last session:** Saturday, July 11, 2026
**Read this first in any new chat.**

---

## The business, in one line

Free live NQ futures trading streams on YouTube -> funnel to **paid NinjaTrader indicators** and **paid 1-on-1 coaching**.

**Deliberately NOT built on prop firm affiliate income.** Considered decision: prop firms go bankrupt, and if you sent people there you own it reputationally. Don't reopen unless there's a new reason.

**Positioning:** *"Read the tape. Not the hype."* No signals, no alerts, no profit-screenshot guru content. This is the whole brand. Everything else follows from it.

---

## DONE

### YouTube — cleaned and rebranded
- `@AlphaTechTraders`, **2,024 subscribers**, clean (no strikes)
- Was dormant since April 2025 (~1 view in 28 days)
- **30 old live streams unlisted** — dated, low-view, branded "TradeByTick" (dead brand, broken affiliate code)
- One public video kept: retitled *"NASDAQ 100: Rewinding the Tape — Reading Price Action for Clues"* (1,031 views — the channel's best asset). Removed the "Accurate Buy Sell Signal" claim.
- Channel description rewritten. Upload Defaults purged of old Apex promo text. Category = Education. Default visibility = Private.

### OBS — built and TEST-VERIFIED
- **AMD HW H.264** encoder (NOT NVENC — this machine has an AMD GPU), 6000 kbps CBR, keyframe **2s**, 1920x1080 @ **30fps**
- **Two scenes:** "Starting Soon" (Canva MP4 loop) and "Live Trading" (Tradovate window capture + EMEET C960 webcam + mic + VLC music playlist)
- **Mic device MUST be "Default"** — selecting Realtek explicitly captured nothing. This cost an hour. Don't repeat.
- Mic filters: Gain (+10dB) -> Compressor (4:1) -> Noise Gate
- Music: **CreatorMix.com** (free, DMCA-safe, cleared for live streams). **Requires "Music by CreatorMix.com" in every description.** ~-26dB.
- **Test: 20+ min, 0 dropped frames, ~2.5% CPU, 6172 kbps. Playback verified clean.**

### Monday's stream — scheduled
Public, Mon July 13, thumbnail uploaded, category Education. Starting Soon screen + 1280x720 thumbnail both built in Canva (black/gold bull).

### Social — created
- Instagram: **@alphatech.traders**
- TikTok: **@alphatech_traders**
- Both: gold bull logo, bio -> YouTube

### Website — LIVE
- **https://alphatech-site-seven.vercel.app**
- Repo: `github.com/aviateatelier/alphatech-site` (private)
- Vercel project: `alphatech-site`
- Local: `C:\Users\patha\OneDrive\Desktop\AlphaTechSite\`
- Single static `index.html`. No build step.
- **Design: macOS window** — traffic lights, sidebar, hairline borders, navy `#000080`, Inter font (SF Pro doesn't exist on Windows — this was a real bug). Hero is a **live animated DOM ladder** showing an absorption event.

---

## NOT DONE — pick up here

### 1. Updated site version is NOT deployed
A newer `index.html` (927 lines) exists with **indicator detail panels, a Discord section, and a Notes/blog section**. Built but never pushed. **The live site is still the older 658-line version.**

### 2. Site blockers

| Issue | Detail |
|---|---|
| **Booking email is DEAD** | Buttons point at `hello@alphatechtraders.com` — doesn't exist. **Swap to `tradersalphatech@gmail.com`.** This is the site's only revenue path and it currently bounces. |
| **Discord link is a placeholder** | Literally the string `DISCORD_INVITE_URL`. Server doesn't exist. |
| **Waitlist stores nothing** | Looks functional, does nothing. Needs Formspree (10 min) or Supabase. |
| **Indicator screenshots missing** | Placeholder frames want: `img/po3-signal-engine.png`, `img/amd-fvg-trade.png`, `img/box-break.png`, `img/daily-range-boxes.png`, `img/atm-risk-lines.png` |
| **Prices are placeholders** | Coaching: $60 / $120 / $250 |

### 3. Domain transfer — IN FLIGHT
- `alphatechtraders.com`: Shopify -> Namecheap
- **The auth code's first char is a lowercase L, not a capital I** — this blocked it for a while
- 5-7 day ICANN wait. Watch `tradersalphatech@gmail.com`.
- **Blocks:** Vercel custom domain AND Google Workspace

### 4. Google Workspace — blocked on transfer
Attempted, failed: *"This domain name is already in use."* Retry once the transfer lands.
Plan: one paid mailbox (`maz@`, $7/mo) + free aliases `hello@` and `support@`. Skip `hr@` — reads as pretend-corporate on a one-person LLC.

### 5. Relaunch video — BIGGEST GAP
Channel is nearly empty. A 60-90s "the channel is back, here's what it is now" video, pinned, tells 2,024 dormant subs and the algorithm what this is. **Not recorded.**

### 6. Indicator licensing — blocks ALL indicator revenue
**Cannot sell until license-protected.** NinjaScript decompiles trivially; one buyer redistributes and the product is dead.
Plan: machine-ID binding on Supabase (Stripe webhook -> license row -> indicator checks machine ID against endpoint) + ConfuserEx obfuscation. Real engineering project.

### 7. Fiverr — abandoned mid-setup
Account exists but username is `@allohatech2024` (typo, unfixable). Decided to make a fresh account later.

---

## The indicators

In `C:\Users\patha\OneDrive\Documents\NinjaTrader 8\bin\Custom\Indicators\`

- **PO3SignalEngine** — 4H Power of Three. Accumulation -> manipulation -> 1-min FVG forms -> **entry fires when price retests that FVG**. Stop at the extreme of the 3 FVG-forming candles + buffer. TP1/TP2 by R:R. Session filter (2AM / London / NY), HTF bias filter, max signals/day.
- **AMDFVGTrade** — same structure, **ATR-scaled** not fixed ticks. Sessions: Sydney/Tokyo/London/NY. Port of a LuxAlgo Pine script.
- **BoxBreak** — zone break signals + **a time stop** (auto-close after N bars).
- **DailyRangeBoxes** — DRH/DIH/DIL/DRL zones. **Levels are MANUAL inputs, not computed.** It's a canvas for your own daily read. Don't sell it as automatic.
- **ATMRiskLines** — reads the live ATM strategy, draws entry/stop/target **with actual dollar risk and R:R on the labels**. Any instrument.
- **DailyBiasZones** — six nested zones (DTH/DRH/DIH/DIL/DRL/DTL). Also manual. Explicitly excluded from the site.

---

## REJECTED — don't re-propose

- **Prop firm comparison site** (like futurestradingwithkellyann.com / propsaver.com). It's an affiliate business; that model was rejected.
- **A "multi-talent" channel** blending trading + aviation vlogs + workouts. The algorithm serves each video to the wrong audience and every video suppresses the next.
- **Starting a fresh YouTube channel.** The 2,024 subs and account age are worth keeping.
- **Profit-screenshot journal.** A wall of green P&L is the guru move and contradicts the brand. If a journal gets built: **journal the setup, not the money, and post the losers.**

---

## Working notes

- **Desktop Commander is connected** — read/write files and run commands on the Windows machine directly.
- **Git:** use Node `execFileSync` with the full path `C:/Program Files/Git/bin/git.exe`. Shell git and chained PowerShell commands get blocked.
- He wants **one step at a time**, with exact click paths. Don't dump multi-step instructions.
- He'll push back on unnecessary detail. Keep it short.

---

## Next, in order

1. **Fix the dead booking email -> Gmail. Push the updated site.** (30 min. It's the only thing between the site and a real coaching inquiry.)
2. **Record the relaunch video.** (Channel is empty; Monday's stream is scheduled.)
3. **Create the Discord**, swap the placeholder link.
4. **Wire the waitlist to Formspree.**
5. Wait on the transfer -> then Vercel domain + Google Workspace.
6. Indicator licensing (the big one).
