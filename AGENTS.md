# Hermes Relay Coordinator instructions

This repository owns the read-only coordination tooling for the Hermes surface
repositories. It is not a product surface and does not own delivery stories.

## Source of truth

- The private Personal Vault Hermes Home hub owns durable product intent and
  shared product or behaviour decisions.
- Each delivery repository owns its local BMad story specification, validation
  record, and status tracker:
  - `hermes-relay-ios`: iOS and macOS Apple-client delivery.
  - `hermes-relay-android`: Android delivery.
  - `hermes-relay-tui`: Textual TUI, ReSpeaker Puck, ESP32 Touch Display, and
    W/K web/iPad delivery.
  - `hermes-relay-home`: Home configuration, arbitration, and service delivery.
  - `hermes-agent`: agent integration tooling; outside BMad delivery scope.
- This repository owns only the roster, validation, and derived portfolio
  report. It must not become a duplicate backlog or status tracker.

## Ticket changes

Route a requested ticket change to the owning repository before editing files:

| Ticket or change | Repository or record |
|---|---|
| iOS or macOS behaviour | `hermes-relay-ios` |
| Android behaviour | `hermes-relay-android` |
| TUI, Puck, ESP32 Touch Display, or W/K behaviour | `hermes-relay-tui` |
| Home configuration, arbitration, or service behaviour | `hermes-relay-home` |
| Agent integration or provider/tooling behaviour | `hermes-agent`; do not create a BMad story here |
| Product intent or a decision shared by multiple surfaces | Private product hub, then separate implementation changes in each affected owner |
| Portfolio report, roster, or validation rule | This repository |

When a ticket changes:

1. Preserve the stable story identity in the owning repository unless the
   product decision explicitly retires or replaces it.
2. Change acceptance criteria, scope, tasks, validation, and status only in
   the owning repository's local artifacts.
3. If the change affects more than one surface, record the durable decision in
   the private product hub and open one focused PR in each affected delivery
   repository. Do not solve the split by editing a sibling tracker.
4. Re-render the portfolio report after the owner changes. Treat a report
   mismatch as a validation failure, not as permission to repair another
   repository's records from here.
5. Keep coordinator changes in their own PR. A coordinator PR may update the
   roster or renderer, but must not smuggle in product scope, private notes, or
   delivery status.

## GitHub Project mirror

GitHub Project #3 is a mechanical mirror of accepted local BMad delivery
records, not the source of truth. Board work is paused unless Amanda explicitly
reopens it. Do not inspect, query, create, edit, move, delete, archive, or
reconcile board items during the pause.

When the board is explicitly reopened, use the owning repository's accepted
local tracker and story index as input. Mirror only the cards belonging to that
repository's registered surfaces, preserve card identity, and map statuses from
the local tracker. Never infer local status from the board, close issues or PRs
as part of a routine mirror, or change a sibling repository's status.

## Safety and review

- Keep the roster path-only. Never put tokens, profile files, audio, private
  vault paths, or story status into it.
- The renderer must remain read-only with respect to its inputs.
- Validate repository-relative artifact references and reject path traversal or
  symlink escape.
- Use deterministic fake repositories in tests; do not require a live Hermes
  endpoint or GitHub access.
- Before a PR, inspect the exact diff and run the focused tests and lint checks.
