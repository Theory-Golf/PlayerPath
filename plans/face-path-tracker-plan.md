# Face Path Tracker Implementation Plan

## Overview
Create a new practice tracker in `Approach/Face Path/` that helps golfers understand and improve their face angle and club path metrics for better direction control on approach shots.

## File Structure
```
Approach/
├── Line Test/
│   └── line-test.html
└── Face Path/
    └── face-path.html (new)
```

## Design System
Match existing trackers with:
- **Background**: Ivory (#fafaf8) and Cream (#f5f4f0)
- **Primary Color**: Forest Green (#2d5016)
- **Accent Color**: Gold (#b8956a)
- **Typography**: Cormorant Garamond (headings), Inter (body)
- **Card Design**: 2px border radius, subtle shadows, mobile-first
- **Progress Indicators**: Linear progress bars with transitions

## Core Features

### 1. Skill Level Selector
Skill levels with different thresholds for each metric:

| Level | Name | Face Angle | Club Path | Face to Path |
|-------|------|------------|-----------|--------------|
| L1 | Foundation | ±8° | ±6° | ±5° |
| L2 | Developing | ±6° | ±5° | ±4° |
| L3 | Competent | ±5° | ±4° | ±3° |
| L4 | Advanced | ±4° | ±3° | ±2.5° |
| L5 | Expert | ±3° | ±2.5° | ±2° |
| L6 | Tour Pro | ±2° | ±2° | ±1.5° |

### 2. Shot Entry Interface
```
┌─────────────────────────────────────┐
│  Shot Entry                         │
├─────────────────────────────────────┤
│  Shot 1/10                          │
│  ████████████░░░░░░░░░░░░░░░ 40%  │
│                                     │
│  ┌─────────────────────────────┐   │
│  │ Face Angle (°): [ -2.5 ]    │   │
│  │ ← Negative  |  Positive →   │   │
│  └─────────────────────────────┘   │
│                                     │
│  ┌─────────────────────────────┐   │
│  │ Club Path (°): [ +1.2 ]     │   │
│  │ ← Negative  |  Positive →  │   │
│  └─────────────────────────────┘   │
│                                     │
│  ┌─────────────────────────────┐   │
│  │ Face to Path: -3.7°        │   │
│  │ (Face - Path calculation)  │   │
│  └─────────────────────────────┘   │
│                                     │
│  [Record Shot]  [Undo]             │
└─────────────────────────────────────┘
```

### 3. Shot List During Session
Display all recorded shots with:
- Shot number
- Face Angle value with direction indicator
- Club Path value with direction indicator
- Face to Path calculated value
- Visual indicator for threshold compliance (green border = met, neutral = missed)

### 4. Real-time Summary
During session, show:
- Total shots recorded
- Current session averages for each metric
- Face Angle within threshold: X/10
- Club Path within threshold: X/10
- Face to Path within threshold: X/10

### 5. Session Complete View
```
┌─────────────────────────────────────┐
│  Session Complete                   │
├─────────────────────────────────────┤
│  Performance Summary                │
│                                     │
│  ┌─────────┬─────────┬─────────┐   │
│  │   FA    │   CP    │   F2P   │   │
│  │  -2.1°  │  +0.8°  │  -2.9°  │   │
│  │   Avg   │   Avg    │   Avg   │   │
│  └─────────┴─────────┴─────────┘   │
│                                     │
│  ┌─────────────────────────────┐   │
│  │ Face Angle:     80% ✓      │   │
│  │ Club Path:      70% ✓      │   │
│  │ Face to Path:   60% ✓      │   │
│  │ (80% threshold benchmark) │   │
│  └─────────────────────────────┘   │
│                                     │
│  Shot Distribution Pattern          │
│  ████████████████░░░░░░░░░░░ 85%  │
│                                     │
│  [New Session]  [View History]      │
└─────────────────────────────────────┘
```

### 6. Historical Performance View
- List all past sessions sorted by date
- Show session averages and threshold compliance rates
- Filter by skill level
- Filter by date range
- Personal best indicators
- Trend visualization

### 7. Benchmark View
Compare against skill level standards:
```
┌─────────────────────────────────────┐
│  Benchmarks                        │
├─────────────────────────────────────┤
│  Your Current Level: L3 Competent  │
│                                     │
│  ┌─────────────────────────────┐   │
│  │ Metric       │ You  │ L3    │   │
│  │ Face Angle   │ 85%  │ 80%   │   │
│  │ Club Path    │ 70%  │ 80%   │   │ 
│  │ Face to Path │ 60%  │ 80%   │   │
│  └─────────────────────────────┘   │
│                                     │
│  Status: ✓ Qualified for Level 3   │
│                                     │
│  Next Milestone: Level 4           │
│  Required: 80% on all metrics      │
└─────────────────────────────────────┘
```

## Data Model

### Session Object
```javascript
{
  id: "timestamp",
  date: "ISO-8601 timestamp",
  skillLevel: 3,
  levelName: "Competent",
  shots: [
    {
      faceAngle: -2.5,      // degrees, negative = closed, positive = open
      clubPath: 1.2,        // degrees, negative = left, positive = right
      faceToPath: -3.7,     // calculated: faceAngle - clubPath
      faceAngleInThreshold: true,
      clubPathInThreshold: false,
      faceToPathInThreshold: true
    }
  ],
  averages: {
    faceAngle: -2.1,
    clubPath: 0.8,
    faceToPath: -2.9
  },
  compliance: {
    faceAngle: 80,  // percentage within threshold
    clubPath: 70,
    faceToPath: 60
  },
  totalShots: 10
}
```

### Skill Level Configuration
```javascript
const SKILL_LEVELS = [
  { level: 1, name: 'Foundation', faceAngle: 8, clubPath: 6, faceToPath: 5 },
  { level: 2, name: 'Developing', faceAngle: 6, clubPath: 5, faceToPath: 4 },
  { level: 3, name: 'Competent', faceAngle: 5, clubPath: 4, faceToPath: 3 },
  { level: 4, name: 'Advanced', faceAngle: 4, clubPath: 3, faceToPath: 2.5 },
  { level: 5, name: 'Expert', faceAngle: 3, clubPath: 2.5, faceToPath: 2 },
  { level: 6, name: 'Tour Pro', faceAngle: 2, clubPath: 2, faceToPath: 1.5 }
];
```

## Implementation Phases

### Phase 1: Core Structure
- Create HTML skeleton with container, header, navigation tabs
- Implement skill level selector
- Build basic shot entry form

### Phase 2: Calculations & State
- Implement Face to Path calculation
- Add threshold validation
- Create real-time display updates

### Phase 3: Session Management
- Implement shot recording
- Add undo functionality
- Create shot list display
- Build session completion logic

### Phase 4: Persistence & History
- localStorage integration
- Historical session list
- Filter functionality

### Phase 5: Visual Enhancements
- Shot distribution visualization
- Trend indicators
- Benchmark comparisons

## Navigation Structure
1. **Test Tab** - Active shot entry and current session
2. **History Tab** - Past sessions with filtering
3. **Benchmarks Tab** - Performance standards and comparisons

## Technical Notes
- Use vanilla HTML/CSS/JS (like Spin Loft tracker)
- Mobile-first responsive design (max-width 480px containers)
- localStorage key: `facePathHistory`
- Single file implementation for easy deployment
- No external dependencies beyond Google Fonts

## Metrics Explained

### Face Angle
- Measures how open or closed the clubface is at impact relative to target line
- Negative value = closed face (tendency to hook)
- Positive value = open face (tendency to slice)
- Tour average: ±3° for elite players

### Club Path
- Direction the club is moving at impact
- Negative value = path left of target (draw/hook tendency)
- Positive value = path right of target (fade/slice tendency)
- Tour average: ±4° for elite players

### Face to Path
- Face Angle minus Club Path
- Determines initial launch direction relative to movement
- Positive value = ball starts right of path (fade/slice)
- Negative value = ball starts left of path (draw/hook)
- Values near zero = square face relative to path
- Critical for understanding shot shape tendencies
