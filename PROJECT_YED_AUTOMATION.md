# Project: yEd Automation & Web Interface

## Vision
An automated pipeline where users (mere mortals) input node/edge/group data into Excel/Web, and a GitHub Action generates a yEd-compatible GraphML file.

## Technical Architecture
- **Language:** Python (for GraphML generation).
- **Core Logic:** Generating nested XML (`<graph>` inside `<node>`) to support yEd's grouping features.
- **Workflow:** Excel/CSV -> Python Script -> .graphml -> yEd.
- **Platform:** GitHub Actions for CI/CD.

## Current Technical Insight
- yEd uses `yfiles` specific XML tags for visual styles.
- Groups are handled by `yfiles_type="group"` and nested `<graph>` tags.
- Avoid using the yEd GUI app in automation due to licensing; focus on pure GraphML generation.

## To-Do
1. Build a Python script to convert a 3-table Excel structure (Nodes, Edges, Groups) to GraphML.
2. Add yFiles visual data (labels, colors) to the XML generation.
3. Create the GitHub Action `.yml`.