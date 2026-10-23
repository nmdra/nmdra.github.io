---
title: "Pele diagram test"
description: "Manual checks for Pele rendering, theme colors, mobile layout, and zoom."
draft: true
showtoc: true
ShowReadingTime: false
ShowPostNavLinks: false
---

This draft page tests Pele with Mermaid code fences. It is excluded from normal production builds.

## Manual checklist

- Use the theme button beside the site title. Check that text, nodes, edges, and chart colors change in both directions.
- Reduce the window to a phone width. Check that labels remain readable and the page does not scroll sideways.
- Check that the flowchart label shows **Input** and **A & B** on separate lines, not raw HTML.
- Use the wide sequence diagram to test zoom and pan when it shrinks to fit. Use the zoom controls, double-click, or Ctrl/Command + mouse wheel.
- Check that every diagram renders independently. The last example is intentionally invalid and should show an error.
- To test the source fallback, disable JavaScript or block `https://cdn.jsdelivr.net/npm/pele@1.9.0/dist/pele.js` in browser developer tools, then reload. Source should remain readable without widening the page.

Diagram links and images are enabled in this integration. Referenced external images can make network requests.

## Flowchart and multiline labels

This horizontal flowchart can switch to a vertical layout on narrow screens.

```mermaid
flowchart LR
    accTitle: Request processing pipeline
    accDescr: Input is validated, then accepted or returned for correction.
    input["Input<br/>A & B"] --> validate{Valid?}
    validate -->|Yes| process[Process request]
    process --> output[Return result]
    validate -->|No| correct[Correct input]
    correct --> input
```

## Sequence diagram

```mermaid
sequenceDiagram
    participant Reader
    participant Hugo
    participant Pele
    Reader->>Hugo: Open page
    Hugo-->>Reader: HTML and escaped diagram source
    Reader->>Pele: Mount diagrams
    Pele-->>Reader: Responsive SVG
    Note over Reader,Pele: Theme colors update through CSS variables
```

## State diagram

```mermaid
stateDiagram-v2
    [*] --> Draft
    Draft --> Review: Submit
    Review --> Draft: Request changes
    Review --> Published: Approve
    Published --> Archived: Retire
    Archived --> [*]
```

## Class diagram

```mermaid
classDiagram
    class Article {
        +String title
        +String body
        +publish()
    }
    class Diagram {
        +String source
        +render()
    }
    Article "1" *-- "many" Diagram : contains
```

## Chart palette

Check the slice and legend colors in both themes.

```mermaid
pie title Content types
    "Engineering" : 40
    "Notes" : 25
    "Projects" : 20
    "Other" : 15
```

## Wide diagram and zoom

At phone width, this sequence diagram should shrink to fit. If zoom controls appear, zoom in and drag to inspect the details. Reset the zoom to see the full diagram again.

```mermaid
sequenceDiagram
    participant Browser
    participant CDN
    participant Parser
    participant Layout
    participant Renderer
    participant Container
    Browser->>CDN: Load Pele module
    CDN-->>Browser: Module ready
    Browser->>Parser: Read Mermaid source
    Parser->>Layout: Parsed diagram model
    Layout->>Renderer: Node and edge positions
    Renderer->>Container: Insert SVG
    Container-->>Browser: Diagram ready
```

## Expected error

The next block is intentionally invalid. It should show a readable error without preventing the diagrams above from rendering. The HTML-like text must not become an image or run JavaScript.

```mermaid
not-a-diagram <img src=x onerror="alert('Diagram source must stay text')">
```
