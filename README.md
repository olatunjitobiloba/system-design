# system-design

System design and architecture analysis, documented with the C4 model.

Take a running system, work out how it is put together from observable
behavior, model it at the right level of abstraction, then state what a
production version would have to change.

## Contents

| Project | System | Levels covered |
|---|---|---|
| [saucedemo-architecture](saucedemo-architecture/) | [saucedemo.com](https://www.saucedemo.com/) | Context, Container, Component |

## Method

Each system gets the same three-step treatment.

1. **Establish what exists.** Inspect the running system and record what you
   find. Network traffic, bundle contents, browser storage, response headers.
   Every claim carries the source it came from, and inference stays labelled as
   inference rather than observation.
2. **Model it in C4.** Diagram at the level of abstraction the question needs:
   the people and external systems around the system, the deployable units
   inside it, then the components within one unit.
3. **State the production gap.** Compare observed behavior against what the
   system would need in order to carry real users and real data, with the
   reasoning for each gap rather than a list of missing features.

## C4 levels

| Level | Question it answers |
|---|---|
| Context | Who and what interacts with this system |
| Container | Which deployable or runnable units make it up |
| Component | What one of those units is built from internally |

Diagrams ship as PNG for viewing and as `.drawio` source for editing, so a later
correction never means redrawing from scratch.

## Tools

draw.io (diagrams.net) with the C4 shape library for notation, Chrome DevTools
for runtime inspection, Wappalyzer for stack detection.
