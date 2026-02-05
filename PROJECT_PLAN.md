# yEd Automation Project Plan

## Project Goal
Make it easy for non-technical users to input network/system data (nodes, edges, groups) and automatically generate yEd-compatible GraphML diagrams.

---

## Phase 1: Requirements & Data Model
**Status:** In Progress

### 1.1 User Research
- [ ] Define target users (personas)
- [ ] Identify pain points with current process
- [ ] Determine technical comfort level expected

### 1.2 Data Model Definition
- [ ] Node properties (what attributes does a node have?)
- [ ] Edge properties (what attributes does a connection have?)
- [ ] Group properties (what attributes does a group/location have?)
- [ ] Connection types catalog (HDMI, Audio, Power, etc.)
- [ ] Nesting rules (can groups contain groups?)

### 1.3 Use Cases
- [ ] Document primary use case (entertainment system)
- [ ] Identify other potential use cases
- [ ] Determine what needs to be flexible vs fixed

---

## Phase 2: Input Design
**Status:** Not Started

### 2.1 Input Format Selection
- [ ] Evaluate options (Web form, CSV, Excel, other)
- [ ] Choose primary input method
- [ ] Design input templates

### 2.2 Validation Rules
- [ ] Define required vs optional fields
- [ ] Edge validation (nodes must exist)
- [ ] Group validation

---

## Phase 3: Core Engine (Python GraphML Generator)
**Status:** Not Started

### 3.1 Parser
- [ ] Read input format(s)
- [ ] Validate data integrity

### 3.2 GraphML Generator
- [ ] Generate base GraphML structure
- [ ] Add yFiles-specific tags for yEd compatibility
- [ ] Handle nested groups
- [ ] Apply visual styles

---

## Phase 4: Output & Styling
**Status:** Not Started

### 4.1 Visual Defaults
- [ ] Default node appearance
- [ ] Edge styles per connection type
- [ ] Group/container styling
- [ ] Color schemes

### 4.2 Layout
- [ ] Layout hints for yEd auto-layout
- [ ] Grouping behavior

---

## Phase 5: Automation & Delivery
**Status:** Not Started

### 5.1 GitHub Actions
- [ ] Trigger on file change
- [ ] Run Python script
- [ ] Output GraphML file

### 5.2 Documentation
- [ ] User guide (how to input data)
- [ ] Technical docs (how the system works)

### 5.3 Web Interface (Optional)
- [ ] Simple form-based input
- [ ] Direct GraphML download

---

## Discovery Notes

### Users & Context

**Target Users:**
- Anyone - technical and non-technical
- Two user types with different needs:
  | User Type | Problem | Solution |
  |-----------|---------|----------|
  | Beginners | yEd has steep learning curve | Template + simple UI, hide complexity |
  | Experts | Repetitive setup tasks every time | Automate the boring parts |

**Key Insight:** Users already understand their systems (nodes, connections, connection types). The problem isn't the data model - it's that Excel makes simple things look complicated. A good UI makes the same data entry painless.

**Current Pain Points (Expert User):**
1. Setting up Excel structure every time (copies old file, deletes data)
2. Going through yEd import wizard every time
3. Visual styling is possible but yEd UX is "un-sexy"

---

### Data Model - Two Levels Identified

**Level 1: Simple Connection Model (Current)**
- Node → Edge → Node, with edge type
- Example: "PS5 connects to AVR via HDMI"
- Sufficient for visualisation

**Level 2: Detailed Port/Compatibility Model (Future?)**
- Devices have specific ports (HDMI-out, TOSLINK-in, etc.)
- Ports have directions (in/out/bidirectional)
- Connections match compatible ports
- Enables: planning, troubleshooting, shopping, documentation

---

### Universal Application

This pattern extends beyond home entertainment:

| Domain | Nodes | Connections | Connection Types |
|--------|-------|-------------|------------------|
| Home entertainment | TV, PS5, AVR | Cables | HDMI, Audio, Ethernet |
| IT infrastructure | Servers, switches, PCs | Network links | Fiber, Cat6, WiFi |
| Organisation | People, Teams, Depts | Reporting lines | Reports to, Collaborates |
| Supply chain | Factories, Warehouses | Transport | Road, Rail, Sea |
| Electrical | Fuse box, Outlets | Wiring | Ring main, Spur |
| Family trees | People | Relationships | Parent, Spouse, Sibling |

**Vision:** A tool that helps anyone map "things and how they connect" - regardless of domain.

---

### Use Cases for Detailed Model
- **Planning:** "Can I connect these two things?"
- **Troubleshooting:** "Why isn't this working?"
- **Shopping:** "What cable/adapter do I need?"
- **Documentation:** "What's actually plugged into what?"

---

## MVP Specification (v1)

### What We're Building

A **web-based tool** where users:

1. **Add nodes** (things they have)
2. **Add connections** between nodes (how things connect)
3. **See a preview** of their diagram
4. **Export to GraphML** for yEd (for advanced features)

### User Workflow

```
┌─────────────────────────────────────────────────────────┐
│  Step 1: ADD NODES                                      │
│  ┌─────────────┐  ┌─────────────┐                       │
│  │ Name: PS5   │  │ Name: TV    │  [+ Add Node]         │
│  │ Group: ___  │  │ Group: ___  │                       │
│  └─────────────┘  └─────────────┘                       │
├─────────────────────────────────────────────────────────┤
│  Step 2: ADD CONNECTIONS                                │
│  ┌──────────────────────────────────────┐               │
│  │ From: [PS5 ▼]  To: [TV ▼]            │ [+ Add]       │
│  │ Type: [HDMI ▼ or type custom]        │               │
│  └──────────────────────────────────────┘               │
├─────────────────────────────────────────────────────────┤
│  Step 3: PREVIEW & EXPORT                               │
│  ┌──────────────────────────────────────┐               │
│  │                                      │               │
│  │     [PS5] ───HDMI──→ [TV]            │               │
│  │                                      │               │
│  └──────────────────────────────────────┘               │
│                                                         │
│  [Download GraphML]  [Copy to Clipboard]                │
└─────────────────────────────────────────────────────────┘
```

### Data Model (MVP)

**Node:**
| Field | Required | Description |
|-------|----------|-------------|
| Name | Yes | Unique identifier, e.g., "PS5", "TV", "AVR" |
| Group | No | Optional grouping, e.g., "Living Room" |

**Connection:**
| Field | Required | Description |
|-------|----------|-------------|
| From | Yes | Source node (must exist) |
| To | Yes | Target node (must exist) |
| Type | Yes | Connection type, e.g., "HDMI" |

**Groups:**
- Created automatically when user types a new group name
- Single level only (no nesting in MVP)
- Future: nested groups

### Connection Type Suggestions

Pre-populated list (user can also type custom):
- HDMI
- Audio Cable
- Ethernet / Cat
- Power
- USB
- Coax
- Optical / TOSLINK
- WiFi / Wireless

### Preview Features

| Feature | MVP | Future |
|---------|-----|--------|
| Auto-layout | Yes | - |
| Zoom/pan | Nice to have | - |
| Drag nodes | Nice to have | - |
| Show groups as containers | Yes | - |
| Edge labels (connection type) | Yes | - |

### Export

- **GraphML file** compatible with yEd
- User opens in yEd for advanced editing (layouts, styling, etc.)

### Out of Scope (Future)

- Device/port compatibility database
- Nested groups
- Visual styling in web UI
- Multiple connection types on same edge
- Import from existing Excel files
- Saving/loading projects

---

## Open Questions (Resolved)

- [x] What's in scope for v1? → Web UI with nodes, connections, preview, GraphML export
- [x] Nested groups? → No, single level for MVP
- [x] Interactive preview? → Nice to have, not required
- [x] Browser-only or yEd needed? → Both: browser for preview, yEd for power features

---

## Technical Decisions

| Component | Choice | Rationale |
|-----------|--------|-----------|
| Web framework | Plain HTML/CSS/JS | No build step, easy to learn, simple deployment |
| Graph library | vis.js | Good docs, auto-layout, handles groups, beginner-friendly |
| Hosting | GitHub Pages | Free, repo integration, CI/CD ready |
| Data input (MVP) | Manual entry via forms | Excel import deferred to v2 |

---

## Implementation Phases

### Phase A: Static UI
- [ ] Create HTML structure (node form, connection form, preview area)
- [ ] Basic CSS styling
- [ ] JavaScript: add/remove nodes and connections
- [ ] Store data in memory (arrays of nodes/connections)

### Phase B: Graph Preview
- [ ] Integrate vis.js
- [ ] Render nodes and edges
- [ ] Auto-layout
- [ ] Show groups as clusters
- [ ] Edge labels (connection types)

### Phase C: GraphML Export
- [ ] Generate GraphML XML structure
- [ ] Add yFiles-specific tags for yEd compatibility
- [ ] Download button
- [ ] Test import in yEd

### Phase D: Polish & Deploy
- [ ] Improve UX (validation, error messages)
- [ ] Responsive design
- [ ] Deploy to GitHub Pages
- [ ] Write user documentation

### Phase E: CI/CD (Future)
- [ ] GitHub Actions workflow
- [ ] Auto-deploy on push to main

### Phase F: Excel Import (Future)
- [ ] Parse Excel files in browser (xlsx.js library)
- [ ] Map to internal data model
- [ ] Validate imported data
