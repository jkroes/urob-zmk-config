# Active Context

This is a living document that captures the current state of work, recent decisions, and immediate next steps. It should be updated frequently.

## Current Focus

The current task is to swap the left and right thumb key bindings on the base layer of the Glove80 keymap. This involves not only swapping the primary bindings but also identifying and updating any related behaviors, macros, or combos that are dependent on these keys being on a specific physical split of the keyboard.

## Recent Changes & Decisions

*Log significant changes, decisions, and their rationale.*

- **[Date]:** Traced and documented the Nix and GitHub Actions build pathways.
  - **Change:** Analyzed the project's files (`Justfile`, `flake.nix`, `build.yaml`, `config/west.yml`, and GitHub Actions workflows) to create a comprehensive understanding of the build processes.
  - **Reason:** To formally document the system's architecture for future reference and to ensure a clear understanding of how firmware is built both locally and in CI.
- **[Date]:** Initial project setup.
  - **Change:** Initialized the Memory Bank documentation.
  - **Reason:** To establish a single source of truth for the project's context, goals, and technical details.

## Next Steps

*What are the immediate, actionable next steps?*

- [x] Update `systemPatterns.md` with the detailed build process analysis.
- [x] Update `techContext.md` to include details about the CI/CD workflows.
- [ ] Identify all bindings, macros, and combos that reference the current left and right thumb key bindings.
- [ ] Swap the primary thumb key bindings in `config/base.keymap`.
- [ ] Update all dependent configurations to reflect the new thumb key locations.
- [ ] Build and flash the firmware to test the changes.

## Open Questions & Blockers

*List any open questions or issues that are blocking progress.*

## Key Learnings & Insights

*Document any new patterns, discoveries, or insights gained during development.*

