
---

# 🔧 **2. Visual Sketch (Markdown / ASCII diagram)**

This diagram fits inside the whitepaper (`docs/auth-flow-mini-whitepaper.md`):

```markdown
## 🔄 Authentication Chain Overview (Concept Diagram)

```text
        +---------------------+
        |   Conditional Access |
        +----------^----------+
                   |
                   |
    +--------------+------------------+
    |         Token Flow Chain        |
    +---------------------------------+
    |   A   |   B   |    C    |   D   |
    |-------|-------|---------|-------|
    |  PRT  |  WAM  | MSAL/   | Edge  |
    |       | Store | OneAuth | WebV2 |
    +---------------------------------+
      |         |         |       |
      v         v         v       v
   Device   Account   App Token  Session
   Trust    Context   Cache      State

```

![Authentication Flow Overview](../diagrams/auth-flow-overview.svg)

![Common Failure Map](../diagrams/failure-map.svg)
