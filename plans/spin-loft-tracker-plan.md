# Approach Spin Loft Tracker Implementation Plan

## Overview
Create a new practice tracker in `Approach/Spin Loft/` that helps golfers monitor and improve their angle of attack and dynamic loft for approach shots across different club groups.

## File Structure
```
Approach/
├── Line Test/
│   └── line-test.html
├── Face Path/
│   └── face-path.html
└── Spin Loft/
    └── spin-loft.html (new)
```

## Design System
Match existing trackers (Face Path, Distance Wedges Spin Loft):
- **Background**: Ivory (#fafaf8) and Cream (#f5f4f0)
- **Primary Color**: Forest Green (#2d5016)
- **Accent Color**: Gold (#b8956a)
- **Typography**: Cormorant Garamond (headings), Inter (body)
- **Card Design**: 2px border radius, subtle shadows, mobile-first
- **Navigation**: Tab-based (Test, History, Benchmarks)

## Club Groups and Benchmark Data

### Club Groups
| Group | Clubs | Typical Use |
|-------|-------|-------------|
| WG | PW, GW (50°-46°) | Full swings 100-125 yards |
| 89 | 9i, 8i (44°-40°) | Full swings 125-150 yards |
| 67 | 7i, 6i (38°-34°) | Full swings 150-175 yards |
| 45 | 5i, 4i (32°-28°) | Full swings 175-200 yards |

### Benchmark Ranges (Target ± Tolerance)

Based on tour data and TrackMan research:

| Group | Angle of Attack | Dynamic Loft |
|-------|-----------------|--------------|
| WG (PW-GW) | -5° to -2° (±2°) | 25° to 35° (±5°) |
| 89 (9i-8i) | -3° to 0° (±2°) | 22° to 30° (±4°) |
| 67 (7i-6i) | -2° to +2° (±2°) | 20° to 28° (±4°) |
| 45 (5i-4i) | -1° to +3° (±2°) | 18° to 25° (±3°) |

### Performance Zones

#### Angle of Attack
| Zone | Classification | Color |
|------|----------------|-------|
| Optimal | Within ±1° of target range | Forest Green (#2d5016) |
| Acceptable | Within benchmark tolerance | Gold (#b8956a) |
| Needs Work | Outside tolerance | Rust (#8b5a3c) |

#### Dynamic Loft
| Zone | Classification | Color |
|------|----------------|-------|
| Optimal | Within ±2° of target range | Forest Green (#2d5016) |
| Acceptable | Within benchmark tolerance | Gold (#b8956a) |
| Needs Work | Outside tolerance | Rust (#8b5a3c) |

## Core Features

### 1. Club Group Selection
```
┌─────────────────────────────────────┐
│  Club Selection                     │
├─────────────────────────────────────┤
│  Select Club Group:                  │
│                                     │
│  ┌─────────┬─────────┐              │
│  │  PW-GW  │  9i-8i  │  L1       │
│  │  46-50° │  40-44° │           │
│  └─────────┴─────────┘              │
│                                     │
│  ┌─────────┬─────────┐              │
│  │  7i-6i  │  5i-4i  │           │
│  │  34-38° │  28-32° │           │
│  └─────────┴─────────┘              │
│                                     │
│  Or select specific club:           │
│  [PW ▼]                             │
└─────────────────────────────────────┘
```

### 2. Shot Entry Interface
```
┌─────────────────────────────────────┐
│  Spin Loft Entry                    │
├─────────────────────────────────────┤
│  Shot 1/5  ████████████░░░░░░ 20%  │
│                                     │
│  Club Group: 9i-8i                  │
│  Target AoA: -3° to 0°              │
│  Target DL: 22° to 30°              │
│                                     │
│  ┌─────────────────────────────┐   │
│  │ Angle of Attack: [-2.5]°   │   │
│  │ ████░░░░░░░░░░░░░░░░░░░░  │   │
│  │ -5°   -3°   Target   0°   +2°│   │
│  └─────────────────────────────┘   │
│                                     │
│  ┌─────────────────────────────┐   │
│  │ Dynamic Loft: [24.5]°       │   │
│  │ ████████░░░░░░░░░░░░░░░░  │   │
│  │ 18°   22°   Target   30°  35°│   │
│  └─────────────────────────────┘   │
│                                     │
│  [Record]  [Clear]                  │
│                                     │
│  Shot 1: AoA ✓ DL ✓                │
└─────────────────────────────────────┘
```

### 3. Shot List with Visual Deviation
```
┌─────────────────────────────────────┐
│  Shots Recorded                     │
├─────────────────────────────────────┤
│  Shot  Club    AoA      DL     Dev │
│  1    9i    -2.5°✓   24.5°✓  ✓✓   │
│  2    9i    -4.2°⚠   26.0°✓  ⚠✓   │
│  3    8i    +1.2°✗   31.2°✗  ✗✗   │
│                                     │
│  Summary: 33% optimal, 33% acc,   │
│           33% needs work           │
│                                     │
│  [Complete Session]                 │
└─────────────────────────────────────┘
```

### 4. Session Summary View
```
┌─────────────────────────────────────┐
│  Session Complete                   │
├─────────────────────────────────────┤
│                                     │
│  Performance by Club Group          │
│                                     │
│  PW-GW (0/2 shots)                 │
│  AoA: --% opt, --% acc, --% work  │
│  DL:  --% opt, --% acc, --% work  │
│                                     │
│  9i-8i (3/2 shots)                 │
│  AoA: 67% ✓, 33% ⚠, 0% ✗         │
│  DL:  100% ✓, 0% ⚠, 0% ✗         │
│                                     │
│  6-7i (0/2 shots)                  │
│  AoA: --% opt, --% acc, --% work  │
│  DL:  --% opt, --% acc, --% work  │
│                                     │
│  4-5i (0/2 shots)                  │
│  AoA: --% opt, --% acc, --% work  │
│  DL:  --% opt, --% acc, --% work  │
│                                     │
│  ═══════════════════════════════    │
│  Overall: 60% optimal               │
│                                     │
│  Tendencies Detected:               │
│  ⚠ Slightly steep AoA tendency     │
│  ✓ Good dynamic loft control        │
│                                     │
│  [New Session]                      │
└─────────────────────────────────────┘
```

### 5. Tendency Analysis
```
┌─────────────────────────────────────┐
│  Systematic Tendencies              │
├─────────────────────────────────────┤
│                                     │
│  Angle of Attack:                   │
│  ▓▓▓▓▓▓▓▓▒▒░░░░░░░░░░░░░░░░░░   │
│  Steep ░░░░░░░░✓Optimal░░░░░ Shallow│
│  Current: Slightly Steep (-0.5°)   │
│                                     │
│  Dynamic Loft:                      │
│  ░░░░▒▒▒▒▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓   │
│  Low ░░░░░░░░✓Optimal░░░░░ High    │
│  Current: Optimal (24.5°)          │
│                                     │
│  Club Comparison:                   │
│  9i: 67% opt  ████████████░░░      │
│  8i: 33% opt  █████░░░░░░░░░░      │
│  7i: --% opt  ░░░░░░░░░░░░░░░░░     │
│  6i: --% opt  ░░░░░░░░░░░░░░░░░     │
│  5i: --% opt  ░░░░░░░░░░░░░░░░░     │
│  4i: --% opt  ░░░░░░░░░░░░░░░░░     │
└─────────────────────────────────────┘
```

## Data Model

### Session Object
```javascript
{
  id: "timestamp",
  date: "ISO-8601 timestamp",
  clubGroup: "89",
  shots: [
    {
      club: "9i",
      angleOfAttack: -2.5,
      dynamicLoft: 24.5,
      aoaInOptimal: true,
      aoaInAcceptable: true,
      aoaStatus: "optimal", // optimal, acceptable, needs-work
      dlInOptimal: true,
      dlInAcceptable: true,
      dlStatus: "optimal",
      aoaDeviation: 0.5, // degrees from nearest target
      dlDeviation: 0.5
    }
  ],
  summary: {
    byClub: {
      "9i": { total: 2, optimal: 1, acceptable: 1, needsWork: 0 },
      "8i": { total: 2, optimal: 0, acceptable: 1, needsWork: 1 }
    },
    byGroup: {
      "89": { aoaOptimal: 50, dlOptimal: 75 }
    },
    tendencies: {
      aoaDirection: "slightly-steep",
      aoaAvg: -1.2,
      dlAvg: 25.3,
      dlDirection: "optimal"
    }
  }
}
```

### Benchmark Configuration
```javascript
const BENCHMARKS = {
  "WG": { // PW-GW
    angleOfAttack: { target: -3.5, tolerance: 2, optimalZone: 1 },
    dynamicLoft: { target: 30, tolerance: 5, optimalZone: 2 }
  },
  "89": { // 9i-8i
    angleOfAttack: { target: -1.5, tolerance: 2, optimalZone: 1 },
    dynamicLoft: { target: 26, tolerance: 4, optimalZone: 2 }
  },
  "67": { // 7i-6i
    angleOfAttack: { target: 0, tolerance: 2, optimalZone: 1 },
    dynamicLoft: { target: 24, tolerance: 4, optimalZone: 2 }
  },
  "45": { // 5i-4i
    angleOfAttack: { target: 1, tolerance: 2, optimalZone: 1 },
    dynamicLoft: { target: 21.5, tolerance: 3, optimalZone: 2 }
  }
};

const CLUBS = {
  "PW": { angle: 46, group: "WG" },
  "GW": { angle: 50, group: "WG" },
  "9": { angle: 42, group: "89" },
  "8": { angle: 40, group: "89" },
  "7": { angle: 38, group: "67" },
  "6": { angle: 34, group: "67" },
  "5": { angle: 32, group: "45" },
  "4": { angle: 28, group: "45" }
};
```

## Implementation Phases

### Phase 1: Core Structure
- Create HTML skeleton with container, header, navigation tabs
- Implement club group selection
- Build benchmark data structure

### Phase 2: Shot Entry
- Create input fields for Angle of Attack and Dynamic Loft
- Implement real-time validation against benchmarks
- Add visual deviation indicators
- Create shot recording and list display

### Phase 3: Session Management
- Implement session completion logic
- Create summary view with club group breakdown
- Add tendency analysis calculations

### Phase 4: History & Persistence
- localStorage integration
- Historical session list
- Filtering by club group and date

### Phase 5: Visual Polish
- Add color-coded visual bars
- Implement tendency visualization
- Add benchmark comparisons

## Key Differentiators from Face Path Tracker

1. **Multiple Club Groups**: Sessions track multiple clubs/groups instead of single metric
2. **Visual Deviation Bars**: Real-time visualization showing shot position relative to target ranges
3. **Tendency Analysis**: Automatic detection of systematic errors (steep/shallow, high/low loft)
4. **Group Summaries**: Performance aggregated by club group
5. **Two Metrics**: Both Angle of Attack and Dynamic Loft tracked independently

## Technical Notes
- Use vanilla HTML/CSS/JS (like Face Path tracker)
- Mobile-first responsive design (max-width 480px containers)
- localStorage key: `approachSpinLoftHistory`
- Single file implementation for easy deployment
- No external dependencies beyond Google Fonts

## Metrics Explained

### Angle of Attack (AoA)
- The vertical angle at which the club approaches the ball
- Negative = descending strike (normal for irons)
- Positive = ascending strike (typical for drivers)
- Tour average for irons: -3° to +2° depending on club

### Dynamic Loft
- The amount of loft the club presents at impact
- Usually 2-5° more than static loft due to shaft bend
- Affects launch angle, spin rate, and trajectory
- Tour average: 20-30° for irons depending on club
