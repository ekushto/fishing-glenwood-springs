# fishing-glenwood-springs

Fly fishing conditions dashboard for the Roaring Fork and Colorado Rivers near Glenwood Springs, CO. Single-file HTML app (`index.html`), no build step, deployed via Netlify at `fishing-glenwood-springs.netlify.app`.

Three tabs: **Conditions** (live USGS gauge data, flow/turbidity charts, active hatches), **Regulations** (CPW data for 5 water segments), **Hatch ID** (conditions banner, decision trees, fly recommendations).

---

## Version history

### Phase A — Data-driven hatch calendar (May 2026)

The original hatch calendar was hand-curated from general guidebook knowledge. Phase A validated every entry against 1,793 parsed Taylor Creek Fly Shop reports (2017–2026) and changed only what the data contradicted.

| Hatch | Original (hand-curated) | Phase A (data-driven) | What the data showed |
|---|---|---|---|
| **Green Drake** | Jun 21 – Jul 21, peak ~10 days | Jun 21 – **Aug 15**, peak ~21 days, featured card | Hatch was cut off 3+ weeks early. Reports show it migrating upstream — Glenwood late June → Basalt mid-July → Upper Fork into August. Old calendar treated it as one brief valley-wide window. |
| **Golden Stone** | Jun 10 – Aug 15 | Jun 10 – **Sep 30** | Shop reports kept recommending golden stone patterns 6 weeks past the old end date, especially Lower and Upper Fork. |
| **Yellow Sally** | Jun 15 – Aug 15 | Jun 15 – **Sep 30** | Appears in reports through September, spreading from Lower Fork in June to all reaches by August. |
| **Skwala** | *(missing)* | Feb 15 – Apr 15 | Early-season stonefly present in spring reports but absent from the calendar. |
| **Winter Stonefly** | *(missing)* | Dec 15 – May 1 | Recurring in late-winter reports. |
| **Cranefly** | *(missing)* | Jul 1 – Sep 15 | Late-summer bank pattern the shop mentions regularly. |

**Unchanged (11 of 14 original entries):** Midge, both BWOs, both Caddis, Stonefly, PMD, Trico, Terrestrial, Red Worm, Egg — the data validated the hand-curated dates.

**Dynamic Green Drake onset detection** (the biggest change, not visible in the table): first-report dates ranged **Jun 17 to Jul 9** across the 9 years — a three-week spread no static date can capture. The app now pulls 60 days of water temp from USGS gauge 09085000 and fires onset when the 7-day rolling mean crosses 52°F after Jun 1, then derives per-reach windows (Lower +0–35d, Middle +14–49d, Upper +28–63d).

*Caveat:* the 52°F trigger is a heuristic, not a forecast. Across the 9 years the 7-day mean at first report ranged 50.5–58.5°F (avg 54.5°F), and the underlying reports are weekly (±7 days of onset noise). Treat it as "drakes likely on or imminent."

Phase A also added local Taylor Creek fly patterns (tagged `local: true`) to all spring dry and nymph decision-tree result nodes.

### Earlier phases

Pre-Phase A history (conditions tab, gauge integration, regulations, spring decision trees) is documented in the local project files (`Fishing_App_Project_Plan 3.0.md`, `RFV-Conditions-Logic-Memo.md`), which are not tracked in this repo.

---

## Planned — Phase B (summer/fall)

1. Season toggle (Spring/Summer/Fall) replacing the hardcoded spring disclaimer
2. Summer dry fly tree — Green Drake as anchor question, then PMD, Golden Stone, Caddis, Terrestrial, Yellow Sally, Trico
3. Summer nymph tree — post-runoff riffle rig, PMD matching, dry-dropper hopper
4. Location-aware Green Drake follow-up using the Phase A onset-derived reach windows
