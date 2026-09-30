# system-design

Architecture analysis and system design work, documented with the C4 model.

Reverse-engineer a running system from observable behavior, model it at the
right level of abstraction, then write up what a production version would have
to change.

## Contents

| Project | System | Levels covered |
|---|---|---|
| [saucedemo-architecture](saucedemo-architecture/) | [saucedemo.com](https://www.saucedemo.com/) | Context, Container, Component |

## How each project is structured

Every project answers three questions in the same order.

1. **What is there.** Evidence first, from DevTools and response
   headers, with an evidence log recording where each claim came from.
2. **How is it shaped.** C4 diagrams at context, container, and component
   level, with every element labeled by type, technology, and role.
3. **What would production require.** The gap between observed behavior and a
   system handling real users and real data, with reasoning for each gap.

Diagrams ship as PNG for viewing and as `.drawio` source for editing, so a
correction never means redrawing from scratch.

## Tools

draw.io (diagrams.net) with the C4 shape library for notation, Chrome DevTools
for runtime inspection, Wappalyzer for stack detection.
