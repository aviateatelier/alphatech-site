# AlphaTech Traders — Session Handoff

**Last session:** Sunday, July 12, 2026
**Read this first in any new chat.**

---

## The business, in one line

Free live NQ futures trading streams on YouTube → funnel to **paid NinjaTrader indicators** and **paid 1-on-1 coaching**.

**Deliberately NOT built on prop firm affiliate income.** Considered decision: prop firms go bankrupt, and if you sent people there you own it reputationally. Don't reopen unless there's a new reason.

**Positioning:** *"Read the tape. Not the hype."* No signals, no alerts, no profit-screenshot guru content. This is the whole brand. Everything else follows from it.

**This positioning has teeth.** Two things got deleted this session specifically because they contradicted it (see "The fake data purge"). Apply the same test to anything new.

---

## Live URLs

| Thing | URL |
|---|---|
| Site | https://alphatech-site-seven.vercel.app |
| Admin (status banner) | https://alphatech-site-seven.vercel.app/status.html |
| Repo | github.com/aviateatelier/alphatech-site (**public**) |
| Vercel project | `alphatech-site` under aviateatelier's projects |
| Local | `C:\Users\patha\OneDrive\Desktop\AlphaTechSite\` |
| Supabase project | `alphatech-traders` — ref `ekignosslynvkjlmpjsl` |
| YouTube channel ID | `UCLXlIjtt7DGQlEcDShrjcoQ` |

---

## What's DONE

### The site — live, and now largely self-updating
Single static `index.html`, no build step. Dark navy + gold. **The macOS-window design from earlier sessions is gone** — replaced with a card layout, sticky nav, Fraunces serif hero.

Sections: hero (+ live NQ tape), countdown widget, curriculum, live sessions (+ economic calendar), coaching, indicators, community, notes, CFTC disclaimer.

### Supabase edge functions (all `verify_jwt: false`)
| Function | Does |
|---|---|
| `next-session` | Reads the **real YouTube schedule** via the Data API. Also detects live-now, and serves the status banner. |
| `nq` | Real CME front-month NQ (`NQ=F`) via Yahoo. Price, change, day range, sparkline, session state, staleness. |
| `calendar` | US economic calendar via TradingView's **undocumented JSON endpoint**. Filtered to high-impact + Fed. |
| `market` | Older ticker feed (Yahoo, 8 symbols). Pre-existing. |
| `truth`, `headlines`, `socialtest` | Live Desk feeds. Pre-existing. |

**Secrets** (Supabase → Edge Functions → Secrets): `YOUTUBE_API_KEY`, `ADMIN_TOKEN`.
`SUPABASE_URL` / `SUPABASE_SERVICE_ROLE_KEY` are injected automatically.

### The countdown is now real
It reads the actual scheduled start time from YouTube. **Change the time in Studio and the site follows** — no code edit. Also flips to a red "LIVE NOW" state when a stream starts, and handles "nothing scheduled" gracefully. Renders in each viewer's own timezone, so it survives the November DST change untouched.

### Status banner + admin page
`status.html` — bookmark it on your phone. Type the ADMIN_TOKEN, write a headline ("Away flying — back Thursday"), optional date, Publish. A gold notice appears above the countdown. "Clear banner" removes it.

Table: `channel_status` (singleton row, public read, service-role write).

### The fake data purge — the big one this session
The site had **two simulated DOM ladders** (`Math.random()`, hardcoded `NQZ6`, fake CVD/delta/absorption). Both deleted.

**Why this matters and why it should not come back:** a fake live order book on a site whose brand is "read the tape, not the hype" is the exact guru move the channel exists to reject. It also showed NQ at ~20,847 when the real contract was ~30,032 — any actual trader spots that instantly.

Replaced with a **real NQ tape panel** in the hero: live price, real change on day, sparkline from genuine 5m bars, session state (Weekend / Globex overnight / US cash session), real session range. Labelled "15-min delayed" and, when the market is shut, the footer reads "Friday's close" instead of pretending to be live.

### Economic calendar
Right column of the sessions section. TradingView's data, rendered in our own styling (their embed widget is an iframe that ignores your design — rejected for that reason). Grouped by day, gold dot = tier-1 (CPI/PPI/NFP/FOMC/GDP/Retail Sales), blue dot = Fed speaker. Shows forecast + previous, flips to actual once released.

### YouTube — cleaned and rebranded
- `@AlphaTechTraders`, **2,024 subscribers**, clean (no strikes)
- 30 old "TradeByTick" streams unlisted; one kept public (1,031 views), retitled, signal-claim removed
- Channel description, upload defaults, category (Education) all reset
- **Monday July 13 stream scheduled — 8:15 AM ET**, public, thumbnail uploaded

### OBS — built and test-verified
- **AMD HW H.264** (NOT NVENC — AMD GPU), 6000 kbps CBR, keyframe 2s, 1080p30
- Two scenes: "Starting Soon" (Canva loop) and "Live Trading" (Tradovate + EMEET C960 + mic + VLC)
- **Mic device must be "Default"** — selecting Realtek explicitly captured nothing. Cost an hour. Don't repeat it.
- Mic chain: Gain (+10dB) → Compressor (4:1) → Noise Gate
- Music: **CreatorMix.com** — requires "Music by CreatorMix.com" in every description. ~-26dB.
- Test: 20+ min, 0 dropped frames, ~2.5% CPU, 6172 kbps. Clean.

### Social
Instagram **@alphatech.traders**, TikTok **@alphatech_traders**. Gold bull logo, bios point to the channel.

---

## Current site schedule block

| | |
|---|---|
| **Monday — Friday** | **08:15 ET** |
| Pre-market read | 08:15 – 09:30 |
| New York session | 09:30 – 16:00 |
| Session recap | 16:00 – 17:00 |

**This table is static text.** Unlike the countdown, it does not read YouTube. If the routine changes, it needs a manual edit.

CPI and PPI both print at 08:30 ET — inside the pre-market window. That's the argument for the 08:15 start.

---

## Typography (as of this session)

Three Google Fonts, loaded in one request with `display=swap`:

| Var | Font | Role |
|---|---|---|
| `--serif` | **Fraunces** (600/700/800, variable optical size) | Voice: hero, headings, logo, tier prices |
| `--sans` | **Archivo** (400/500/600/700) | Body: paragraphs, buttons, nav, inputs |
| `--mono` | **IBM Plex Mono** (400/500/600) | Data: prices, times, calendar, countdown, disclaimer |

The three-tier split is deliberate — serif = opinion, sans = explanation, mono = machine-produced data. It's why the numbers read as credible.

(Old notes mention **Inter** — stale. Replaced in the redesign.)

**Known weakness: mobile type sizing.** The hero scales with `vw`, but most other text sits at desktop sizes on a phone. Next thing to fix.

---

## What's NOT done — pick up here

### 1. Booking email
Buttons now point at `tradersalphatech@gmail.com` (fixed last session — the old `hello@alphatechtraders.com` didn't exist). Still the site's only revenue path.

### 2. Blockers
| Issue | Detail |
|---|---|
| **Discord link is a placeholder** | Literally `DISCORD_INVITE_URL`. Server doesn't exist. |
| **Indicator screenshots missing** | Wants `img/po3-signal-engine.png`, `amd-fvg-trade.png`, `box-break.png`, `daily-range-boxes.png`, `atm-risk-lines.png` |
| **Prices are placeholders** | Coaching tiers at $60 / $120 / $250 |
| **Waitlist** | Now wired to Supabase (was a stub). Working. |

### 3. Domain transfer — in flight
- `alphatechtraders.com` transferring **Shopify → Namecheap**
- **The auth code's first character is a lowercase L, not a capital I**
- 5–7 day ICANN wait. Watch `tradersalphatech@gmail.com`.
- **Blocks:** custom domain on Vercel, AND Google Workspace

### 4. Google Workspace — blocked on the transfer
Failed with *"This domain name is already in use"* because the domain is mid-transfer. **Retry once it lands.**
Plan: one paid mailbox (`maz@`, $7/mo) + free aliases for `hello@` / `support@`. Skip `hr@` — reads as pretend-corporate on a one-person LLC.

### 5. The relaunch video — STILL THE BIGGEST GAP
The channel is nearly empty. A 60–90 second "the channel is back, here's what it is now" video, pinned, is what tells 2,024 dormant subs and the algorithm what AlphaTech Traders is. **Not recorded.** Monday's stream is scheduled.

### 6. Indicator licensing — blocks all indicator revenue
**Cannot sell until license-protected.** NinjaScript decompiles trivially; one buyer redistributes and the product is dead.
Plan: machine-ID binding on Supabase (Stripe webhook → license row → indicator checks machine ID against an endpoint) + ConfuserEx obfuscation. Real engineering project.

### 7. Fiverr — abandoned mid-setup
Username is `@allohatech2024` (typo, unfixable). Create a fresh account later.

---

## The indicators (source read, content written)

In `C:\Users\patha\OneDrive\Documents\NinjaTrader 8\bin\Custom\Indicators\`

- **PO3SignalEngine** — 4H Power of Three. Accumulation → manipulation → 1-min FVG forms → **entry fires when price retests that FVG**. Stop at the extreme of the 3 FVG-forming candles + buffer. TP1/TP2 by R:R. Session filter, HTF bias filter, max signals/day.
- **AMDFVGTrade** — same structure, **ATR-scaled** instead of fixed ticks. Sydney/Tokyo/London/NY. Port of a LuxAlgo Pine script.
- **BoxBreak** — zone break signals + **a time stop** (auto-close after N bars).
- **DailyRangeBoxes** — DRH/DIH/DIL/DRL zones. **Levels are manual inputs, not computed.** Don't sell it as automatic.
- **ATMRiskLines** — reads the live ATM strategy, draws entry/stop/target **with actual dollar risk and R:R on the labels**. Any instrument.
- **DailyBiasZones** — six nested zones. Also manual. Explicitly excluded from the site.

---

## Rejected ideas — don't re-propose

- **Prop firm comparison site.** Affiliate business; that model was deliberately rejected.
- **A "multi-talent" channel** blending trading + aviation + workouts. The algorithm serves each video to the wrong audience.
- **Starting a fresh YouTube channel.** The 2,024 subs and account age are worth keeping.
- **Profit-screenshot journal.** The guru move. If a journal gets built: **journal the setup, not the money, and post the losers.**
- **Simulated market data of any kind.** See "the fake data purge." Two ladders died for this.
- **Real-time CME data (Barchart / TradingView paid / CME direct).** Investigated this session. It's a **licensing wall, not a technical one** — real-time futures requires an exchange redistribution agreement plus per-user CME fees, several hundred $/mo minimum. The 15-min delayed feed is honest and costs nothing. Revisit only if there's a members-only page behind a login.
- **Level 2 / DOM depth data.** Same wall. No free or scrapeable source exists. This is why the ladder can never be made real.

---

## Working notes

- **Desktop Commander is connected** — read/write files and run commands on the Windows machine. Use the full git path (`C:\Program Files\Git\bin\git.exe`); shell git and chained PowerShell get blocked.
- **NEVER use PowerShell `Set-Content -Encoding UTF8`** on index.html. It writes a **BOM** and double-encodes every em-dash, arrow, and checkmark (`—` becomes mojibake). This corrupted the file once this session — 139 characters destroyed, required a git revert. Use `[System.IO.File]::WriteAllText($p, $t, (New-Object System.Text.UTF8Encoding($false)))` — the `$false` is the no-BOM constructor. Or just use `edit_block`.
- **The file uses LF line endings, not CRLF.** `edit_block` multi-line matches will fail if you assume CRLF.
- **The Vercel webhook is unreliable.** It has silently missed pushes 3+ times across two sessions. Always verify the deploy landed; if not, an empty commit (`git commit --allow-empty`) forces it. Worth disconnecting/reconnecting the GitHub integration properly.
- He wants **one step at a time**, with exact click paths. Don't dump multi-step instructions.
- He'll push back on unnecessary detail. Keep it short.
- **He is right to push back on data accuracy.** Twice this session he questioned numbers and was correct both times — it surfaced a wrong prev-close (change % was 7x inflated) and a stale quote presented as live. Verify before defending.

---

## What I'd do next, in order

1. **Typography pass — mobile especially.** Desktop type is solid; mobile is the weak point.
2. **Record the relaunch video.** The channel is empty and Monday's stream is scheduled.
3. **Create the Discord**, swap the placeholder link.
4. Wait on the domain transfer → then Vercel custom domain + Google Workspace.
5. Indicator screenshots + real prices.
6. Indicator licensing (the big one).
