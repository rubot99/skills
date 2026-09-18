# skills

A growing library of [Claude Code](https://claude.com/claude-code) Skills — reusable, packaged instruction sets that extend Claude Code for specific tasks.

## Structure

Skills are grouped into category directories. Each skill lives in its own folder containing a `SKILL.md` (name, description, and instructions for Claude) plus any supporting reference files it needs — templates, examples, or scripts.

```
<category>/
  <skill-name>/
    SKILL.md
    <supporting files>
```

## Available Skills

### architecture

| Skill | Description |
|---|---|
| [`create-adr`](architecture/create-adr) | Draft, number, and file Architecture Decision Records (ADRs), and review or critique existing ADR drafts, for a fintech/payments engineering context. |
| [`c4-diagram`](architecture/c4-diagram) | Generate C4-model architecture diagrams (System Context, Container, or Component level) as Mermaid diagrams, following a fixed notation standard. |
| [`sequence-diagram`](architecture/sequence-diagram) | Generate sequence diagrams (call/message flows between actors, services, and systems over time) as Mermaid diagrams, following a fixed notation standard. |

### engineering

| Skill | Description |
|---|---|
| [`pr-description`](engineering/pr-description) | Draft PR descriptions and commit messages in a fixed, reviewable format (ticket link, risk assessment, testing notes) instead of an ad hoc structure each time. |

## License

MIT — see [LICENSE](LICENSE).
