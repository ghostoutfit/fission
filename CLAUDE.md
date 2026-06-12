# fission — Fission Digital Model

Standalone single-file sim deployed to `ghostoutfit.github.io/fission/`.
Derived from the canonical `v13/index.html` in the `protons` repo — **fission tab only**, chain reaction fully removed.
Vanilla HTML + CSS + JS only. No build step — edit `index.html` directly.

To run locally: `python3 -m http.server` in this directory, then open `localhost:8000/`.

## Differences from canonical v13

- Chain Reaction tab and all its code removed (see below)
- `currentTab` is a `const` `'fission'` — never changes
- `applyTab()` simplified to fission-only init
- Image paths use `images/` (no `../images/` parent prefix)

## What was removed (chain reaction code)

Everything related to chain reaction is gone:
- `chainNeutrons`, `chainRAF`, `enrichSites`, `chainParticleOffsets`
- `drawChainOverlay`, `drawChainBarChart`, `updateChain`, `ensureChainLoop`
- `triggerEnrichSite`, `emitChainNeutrons`, `pickChainSite`
- All `CHAIN_*` constants

Do not re-add these — use the `chain-reaction` repo for that functionality.

## Physics constants

```javascript
const PARTICLE_RADIUS = 10;
const coulombStrength = 150;   // distinct from v11 (139) and v14 (400)
```

## Phase machine

Fission animation parameterized by scrub fraction `t ∈ [0, 1]`:

```
NEUTRON_FRAC = 0.20   // neutron approach
PH1_END      = 0.30   // wobble buildup
PH2_END      = 0.375  // elongation peak
ANIM_T_END   = 0.47   // daughter ejection ends
```

No frame buffer — scrubbing is phase-indexed via `currentScrubFrac`. `renderAtFrac(frac)` reconstructs geometry analytically.

## Speed slider

`#playbackSpeed` — min 0.1, max 2.05, step 0.05, default **1.0**.
Blue tick mark at default position; snaps within ±0.10 of 1.0.

## Key globals

```javascript
let staticFissAngle = 0;       // reset on NEW/Clear/Go; randomized at neutron impact
let dragRotY = 0, dragRotX = 0;  // nucleus drag accumulators; cleared on mouseup
let wobbleMode;                // 'dip1' / 'dip2' / 'dip3' / 'stable'
const NORMAL_DURATION = 5000;  // ms — fission playback length
```

## Images

```
images/
  favicon.png / favicon.svg
  Turtle.png / Rabbit.png / Stopwatch.png
  logo-placeholder.png
```

## Deployment

GitHub Pages from `main` branch root. Push to `origin` to deploy.
Remote: `https://github.com/ghostoutfit/fission.git`

## Cross-sim search

```bash
# Compare with canonical v13
grep -n "functionName" index.html /path/to/protons/v13/index.html
```

Changes to canonical `v13/index.html` in the protons repo should generally be ported here (fission-relevant parts only). They are not auto-synced.
