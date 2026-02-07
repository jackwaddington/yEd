# yEd Graph Builder

One of my favourite software packages <3 https://www.yworks.com/products/yed

## The Problem

I was amazed with relational databases, frustrated with the amount of clicks it took to rebuild diagrams, and surprised at the cost of software to create maps from rectangular data.

The software packages that seemed to be able to work with tables of data were Microsoft Visio and Lucidchart, but they were expensive. https://app.diagrams.net/ was accessible, but could not take in tables of data.

There was a bit of a hump to learn yEd. Even as an expert, I found myself doing the same repetitive tasks every time to create a graph.

## The Solution

A simple web-based tool where anyone can:

1. **Add nodes** (things they have)
2. **Add connections** between nodes (how things connect)
3. **See a preview** of their diagram instantly
4. **Export to GraphML** for yEd (for advanced editing)

**Try it:** Open `index.html` in your browser.

---

## How It Works

The entire application is a single HTML file with three sections:

```
┌─────────────────────────────────────────────────────────┐
│  <head>                                                 │
│    └── <style>  ←── CSS (how things LOOK)               │
│                     Colors, spacing, fonts, layout      │
├─────────────────────────────────────────────────────────┤
│  <body>                                                 │
│    └── HTML elements ←── STRUCTURE (what things ARE)    │
│        Forms, buttons, divs, inputs                     │
├─────────────────────────────────────────────────────────┤
│  <script>                                               │
│    └── JavaScript ←── BEHAVIOR (what things DO)         │
│        Add node, remove node, draw graph, export        │
└─────────────────────────────────────────────────────────┘
```

### The Magic: vis.js

One line loads a powerful graph visualization library:

```html
<script src="https://unpkg.com/vis-network/standalone/umd/vis-network.min.js"></script>
```

This loads ~300KB of graph visualization code from the internet. We just use it:

```javascript
// Create data containers
let visNodes = new vis.DataSet();
let visEdges = new vis.DataSet();

// Create the graph (this one line does all the drawing!)
network = new vis.Network(container, {nodes: visNodes, edges: visEdges}, options);

// Add a node - vis.js automatically redraws
visNodes.add({id: 1, label: 'PS5'});
```

### How the Pieces Talk to Each Other

| When you...          | JavaScript...              | Which updates...              |
|----------------------|----------------------------|-------------------------------|
| Click "Add Node"     | Adds to `nodes` array      | The node list + vis.js graph  |
| Click "Add Connection"| Adds to `connections` array| The connection list + graph   |
| Click "Download"     | Builds XML string          | Creates a file download       |

### Data Model

**Nodes:**
```javascript
{ id: 1, label: 'PS5', group: 'Living Room' }
```

**Connections:**
```javascript
{ from: 1, to: 2, type: 'HDMI' }
```

That's it. Simple arrays of objects.

---

## Technology Stack

| Component      | Choice                | Why                                      |
|----------------|-----------------------|------------------------------------------|
| Web framework  | Plain HTML/CSS/JS     | No build step, easy to learn, portable   |
| Graph library  | vis.js                | Good docs, auto-layout, handles groups   |
| Export format  | GraphML               | Industry standard, yEd compatible        |
| Hosting        | GitHub Pages          | Free, integrates with repo               |

---

## The Universal Pattern

This tool works for any domain where you have "things and how they connect":

| Domain              | Nodes                    | Connections      | Types                        |
|---------------------|--------------------------|------------------|------------------------------|
| Home entertainment  | TV, PS5, AVR             | Cables           | HDMI, Audio, Ethernet        |
| IT infrastructure   | Servers, switches, PCs   | Network links    | Fiber, Cat6, WiFi            |
| Organisation        | People, Teams, Depts     | Reporting lines  | Reports to, Collaborates     |
| Supply chain        | Factories, Warehouses    | Transport        | Road, Rail, Sea              |
| Electrical          | Fuse box, Outlets        | Wiring           | Ring main, Spur              |
| Family trees        | People                   | Relationships    | Parent, Spouse, Sibling      |

---

## Files in This Repository

| File                                    | Purpose                                      |
|-----------------------------------------|----------------------------------------------|
| `index.html`                            | The web application                          |
| `PROJECT_PLAN.md`                       | Detailed project planning and decisions      |
| `yEd Tom entertainment system.xlsx`    | Example: Excel data for yEd import           |
| `*.pdf`                                 | Example: Visualizations created in yEd       |

---

## Learning Resources

- **vis.js Network Examples:** https://visjs.github.io/vis-network/examples/
- **vis.js Documentation:** https://visjs.github.io/vis-network/docs/network/
- **GraphML Specification:** https://en.wikipedia.org/wiki/GraphML
- **yEd Graph Editor:** https://www.yworks.com/products/yed

---

## Future Ideas

- [ ] Visual group boxes around related nodes
- [ ] Import from existing Excel files
- [ ] Save/load projects to browser storage
- [ ] Deploy to GitHub Pages
- [ ] CI/CD pipeline

See `PROJECT_PLAN.md` for detailed planning notes.
