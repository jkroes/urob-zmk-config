# Active Context

This is a living document that captures the current state of work, recent decisions, and immediate next steps. It should be updated frequently.

## Current Focus

The current task is to swap the left and right thumb key bindings on the base layer of the Glove80 keymap. This involves not only swapping the primary bindings but also identifying and updating any related behaviors, macros, or combos that are dependent on these keys being on a specific physical split of the keyboard.

## Recent Changes & Decisions

*Log significant changes, decisions, and their rationale.*

- **[Date]:** Initial project setup.
  - **Change:** Initialized the Memory Bank documentation.
  - **Reason:** To establish a single source of truth for the project's context, goals, and technical details.

## Next Steps

*What are the immediate, actionable next steps?*

- [ ] Identify all bindings, macros, and combos that reference the current left and right thumb key bindings.
- [ ] Swap the primary thumb key bindings in `config/base.keymap`.
- [ ] Update all dependent configurations to reflect the new thumb key locations.
- [ ] Build and flash the firmware to test the changes.

## Open Questions & Blockers

*List any open questions or issues that are blocking progress.*

- What is the most effective way to trace all dependencies of the thumb key bindings?

## Key Learnings & Insights

*Document any new patterns, discoveries, or insights gained during development.*

-
