# Hermes Relay Coordinator

The coordinator provides a read-only portfolio view across the Hermes delivery
repositories. It reads each repository's local BMad registration, story index,
and status tracker, validates that those records join cleanly, and renders a
derived Markdown report.

The ownership boundary is intentional:

| Concern | Owner |
|---|---|
| Durable product intent and shared decisions | Private Personal Vault Hermes Home hub |
| iOS delivery stories, specifications, validation, and status | `hermes-relay-ios` |
| Android delivery stories, specifications, validation, and status | `hermes-relay-android` |
| TUI, Puck, ESP32 Touch Display, and W/K delivery stories, specifications, validation, and status | `hermes-relay-tui` |
| Home configuration, arbitration, and service delivery stories, specifications, validation, and status | `hermes-relay-home` |
| Agent integration tooling | `hermes-agent` (outside BMad delivery scope) |
| Cross-repository read-only roll-up | This repository |

The coordinator is not a backlog and is not a second status authority. A ticket
change belongs in the owning delivery repository. A product or shared-behaviour
decision belongs in the private product hub. The report here is evidence for
orientation and review; it never writes to a surface repository.

## Render the portfolio report

The standard roster assumes the five delivery repositories are adjacent to this
checkout and contains paths only:

```bash
uv run python scripts/render_surface_status.py \
  --config surface-repositories.yaml \
  --output surface-status-report.md
```

For worktrees or another checkout layout, pass all five repositories explicitly:

```bash
uv run python scripts/render_surface_status.py \
  --repo tui=../hermes-relay-tui-worktrees/federated-bmad-ownership \
  --repo ios=../hermes-relay-ios-worktrees/federated-bmad-ownership \
  --repo android=../hermes-relay-android-worktrees/federated-bmad-ownership \
  --repo home=../hermes-relay-home-worktrees/federated-bmad-ownership \
  --repo agent=../hermes-agent-worktrees/federated-bmad-ownership \
  --output surface-status-report.md
```

The command rejects incomplete rosters by default. Use `--allow-partial` only
for a deliberate diagnostic run. A generated report is disposable and should
not be committed as status input.

## Development

The coordinator requires Python 3.14. Run the focused checks with:

```bash
uv run --no-project --python 3.14 \
  --with pytest --with PyYAML --with ruff \
  -- python -m pytest -q
uv run --no-project --python 3.14 \
  --with pytest --with PyYAML --with ruff \
  -- ruff check scripts tests
```

No live Hermes endpoint, GitHub Project, token, product-hub path, or private
note is required to test the renderer.
