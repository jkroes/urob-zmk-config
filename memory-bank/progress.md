# Project Progress

This document tracks the overall progress of the project, including what's working, what's left to do, and any known issues.

## What's Working

- **Initial QWERTY Layout:** The base layer has been converted to a standard QWERTY layout.
- **Build Environment:** The Nix-based local build environment is fully functional and documented.
- **CI/CD:** The dual GitHub Actions workflows (standard and Nix-based) are in place for automated firmware builds.
- **Core `urob/zmk-config` Features:** The advanced features from the original repository (homerow mods, combos, etc.) are present but may need adaptation.

## What's Left to Build

*Outline the remaining work, organized by feature or component.*

- **Base Layer Customization:**
  - [ ] Swap left and right thumb key bindings.
  - [ ] Update all dependent behaviors, macros, and combos.
- **Cross-Platform Shortcuts:**
  - [ ] Implement consistent shortcuts for macOS and Windows.
- **Ergonomic Layout Migration:**
  - [ ] Research and select an ergonomic layout (e.g., Colemak-DH, Miryoku).
  - [ ] Implement the selected ergonomic layout.

## Known Issues & Bugs

*Track any known issues, bugs, or technical debt.*

- **Issue:** The current QWERTY layout is a temporary solution and not yet optimized for ergonomics.
  - **Status:** Open
  - **Priority:** Medium
- **Issue:** Many keybindings and macros may have implicit dependencies on the physical location of keys (left vs. right split). Swapping the thumb keys is likely to break some functionality until all dependencies are updated.
  - **Status:** Open
  - **Priority:** High

## Evolution of Decisions

*Summarize how key decisions have evolved over the course of the project.*

- **Initial Decision:**
- **Change:**
- **Reason:**
