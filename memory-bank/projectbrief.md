# Project Brief

This document outlines the high-level goals, scope, and constraints of the project. It serves as the foundational understanding for all development work.

## Core Mission

This project is a personal fork of `urob/zmk-config`, adapted to create a customized ZMK firmware configuration for the Glove80 keyboard. The primary purpose is to tailor the keyboard's behavior to specific personal needs and preferences, leveraging the powerful features of ZMK firmware.

## Key Objectives

*List the main, measurable goals.*

- Adapt the existing `urob/zmk-config` to be fully compatible with the Glove80 keyboard hardware.
- Implement and refine a personal keymap that optimizes workflow and comfort.
- Leverage advanced ZMK features such as "timeless" homerow mods, combos, and smart layers for an efficient and ergonomic typing experience.
- Maintain a clean, automated, and reproducible build environment using Nix.

## Scope

*Define the boundaries of the project. What is in scope and what is out of scope?*

### In Scope

- Full adaptation of the `urob/zmk-config` for the Glove80 keyboard.
- Initial implementation of a standard QWERTY layout for the alpha keys.
- Retention and customization of advanced features like homerow mods, combos, smart layers, and the leader key.
- Future migration to a more ergonomic layout after the initial configuration is stable.

### Out of Scope

- Support for keyboards other than the Glove80.
- Major changes to the core ZMK firmware, beyond what's necessary for the custom configuration.

## Target Audience

The primary and sole user of this project is the developer. The configuration is tailored for personal use and workflow optimization.

## Constraints & Assumptions

*List any known constraints (technical, budget, timeline) and assumptions made.*

- **Hardware Constraint:** The configuration is tightly coupled to the Glove80 hardware layout.
- **Configuration Complexity:** Keys and layers are highly interdependent. Moving a key binding from one physical split of the keyboard to the other may require updating associated modifiers (e.g., from left-shift to right-shift) or swapping layer definitions.
- **Assumption:** The `urob/zmk-config` serves as a stable and feature-rich foundation for this custom configuration.
- **Phased Rollout:** The initial focus is on a functional QWERTY layout before transitioning to a more ergonomic one.
