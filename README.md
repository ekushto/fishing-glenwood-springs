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

### Phase B — Season toggle + summer/fall decision trees (July 2026)

Hatch ID tab now covers the full season. A Spring/Summer/Fall toggle auto-selects by date (Dec–Feb maps to Spring) and can be tapped to override. Four new decision trees, all pattern choices from seasonal cuts of the 9-year dataset:

- **Summer dry** (13 results) — Green Drake anchor question with per-reach follow-up: pick Lower/Middle/Upper and the result includes live onset-window status ("on now — peak", "not here yet, expected Jul 20–Aug 24", "finished, try upstream"). Plus PMD dun/emerger (fixing the missing PMD dry node), Golden Stone, Yellow Sally, caddis ×2, hopper, ant/beetle, summer midge/BWO.
- **Summer nymph** (7 rigs) — post-runoff riffle rig (Freestone Emerger + Biot Baetis, the dataset's top two summer flies), stonefly, worm (off-color water), PMD, baetis, dry-dropper ×2.
- **Fall dry** (7 results) — anchored on fall BWO (97% of fall reports); midge ×2, late caddis, PMD/Rusty Spinner, late terrestrial.
- **Fall nymph** (5 rigs) — baetis, searching, deep-slot, midge, and an egg rig with a fish-below-the-redds ethics note.

**Trico was dropped from the planned summer tree**: 5 mentions in 1,793 reports across 9 years — the shop effectively never calls it on the Fork.

Also: removed two unreachable BWO result nodes from the original spring tree (dead since v1; duplicated reachable content), and the spring "not a spring hatch" node now points at the Summer toggle.

Verified by a jsdom harness (`test_phase_b.mjs`, local-only) that walks all 55 terminal paths across the six trees and the full Green Drake reach-status matrix — 572 checks.

### Earlier phases

Pre-Phase A history (conditions tab, gauge integration, regulations, spring decision trees) is documented in the local project files (`Fishing_App_Project_Plan 3.0.md`, `RFV-Conditions-Logic-Memo.md`), which are not tracked in this repo.

---

## Known gaps

- `guide-review.html` doesn't reflect Phase A or Phase B logic yet
- Chart touch interaction: value boxes should track the touched date, reverting to live when released
- Regulations data is hardcoded — manual CPW update each March
- Fall fly rankings drawn from the same 9-year dataset but not yet guide-reviewed
