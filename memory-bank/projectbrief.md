# Project Brief

This document outlines the high-level goals, scope, and constraints of the project. It serves as the foundational understanding for all development work.

## Core Mission

This project configures and builds customized ZMK firmware for the Glove80 keyboard. It is a fork of `urob/zmk-config`, adapted for personal use by Justin Kroes.  

## Key Objectives

*List the main, measurable goals.*

- Learn about the structure and content of the original GitHub repository `urob/zmk-config`, in order to guide adaptation and future development of this forked project.
- Implement and refine a personal keymap that optimizes workflow and comfort.
- Leverage advanced ZMK features such as "timeless" homerow mods, combos, and smart layers for an efficient and ergonomic typing experience.
- Maintain a clean, automated, and reproducible build environment using Nix.

## Scope

*Define the boundaries of the project. What is in scope and what is out of scope?*

### In Scope

- Changes to files in `config`.  
- Support for the Glove80 split keyboard.

### Out of Scope

- Changes to files outside of `config`.  
- Support for keyboards other than the Glove80.

## Target Audience

The primary and sole user of this project is the developer. The configuration is tailored for personal use and workflow optimization.

## Constraints & Assumptions

*List any known constraints (technical, budget, timeline) and assumptions made.*

- **Hardware Constraint:** The configuration is tightly coupled to the Glove80 hardware layout.
- **Configuration Complexity:** Keys are highly interdependent on layers and other keys. Moving a key binding from one physical split of the keyboard to the other may require updating associated modifiers (e.g., from left-shift to right-shift), swapping layer definitions, or modifying other bindings. 
- **Assumption:** The original project by `urob` serves as a stable and feature-rich foundation for this custom configuration.
