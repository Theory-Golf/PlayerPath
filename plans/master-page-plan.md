# Master Page Plan - PlayerPath

## Overview
Create a central hub page (`index.html`) that organizes all practice links by Skill category with two subcategories: Skill Development and Skill Assessment.

## File Structure

```
PlayerPath/
├── index.html          (NEW - Master page)
├── Approach/
│   └── Line Test/
│       └── line-test.html
├── Distance Wedges/
│   ├── Spin Loft/
│   │   └── index.html
│   ├── Stack Wedge/
│   │   ├── index.html
│   │   └── wedge-master.html
│   └── Wedge Club Speed/
│       └── index.html
├── Driving/
│   └── (empty - no practices)
└── Putting/
    ├── Speed Ratio/
    │   └── speed-control-practice.html
    └── Stack Putting/
        └── stack-putting.html
```

## Page Structure

### Header
- Title: "PlayerPath Practice Hub"
- Brief description

### Skills Sections (4 total)

#### 1. Approach
- **Skill Development:** (none)
- **Skill Assessment:** Line Test → [`Approach/Line Test/line-test.html`](Approach/Line%20Test/line-test.html)

#### 2. Distance Wedges
- **Skill Development:**
  - Spin Loft → [`Distance Wedges/Spin Loft/index.html`](Distance%20Wedges/Spin%20Loft/index.html)
  - Club Speed → [`Distance Wedges/Wedge Club Speed/index.html`](Distance%20Wedges/Wedge%20Club%20Speed/index.html)
- **Skill Assessment:**
  - Stack Wedge → [`Distance Wedges/Stack Wedge/index.html`](Distance%20Wedges/Stack%20Wedge/index.html) (primary)
  - Stack Wedge (Master) → [`Distance Wedges/Stack Wedge/wedge-master.html`](Distance%20Wedges/Stack%20Wedge/wedge-master.html) (alternative)

#### 3. Driving
- **Skill Development:** Coming Soon
- **Skill Assessment:** (none)

#### 4. Putting
- **Skill Development:**
  - Speed Ratio → [`Putting/Speed Ratio/speed-control-practice.html`](Putting/Speed%20Ratio/speed-control-practice.html)
- **Skill Assessment:**
  - Stack Putting → [`Putting/Stack Putting/stack-putting.html`](Putting/Stack%20Putting/stack-putting.html)

## Design Considerations

1. **Navigation:** Clear visual distinction between Skill Development and Skill Assessment sections
2. **Styling:** Match existing aesthetic of practice pages (if any)
3. **Responsive:** Work on desktop and mobile
4. **Maintainability:** Easy to add new practices in the future
   - Use a data structure (JSON or array) to define practices for easy updates
   - Comment sections clearly for future additions
   - Consider a template-like structure for adding new skills

## Next Steps
1. Create the HTML structure
2. Add CSS styling
3. Test all links work correctly
