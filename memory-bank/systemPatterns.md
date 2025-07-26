# System Patterns

This document describes the architecture and recurring design patterns used throughout the system. It's a guide for maintaining architectural consistency.

## System Architecture Overview

The system is a ZMK firmware configuration for the Glove80 keyboard, managed as a personal fork of `urob/zmk-config`. The architecture is composed of several key components:

- **ZMK Firmware:** The core firmware that runs on the keyboard.
- **`west` Manifest (`config/west.yml`):** Manages the ZMK firmware and its module dependencies, pinning everything to specific versions for reproducibility.
- **Nix (`flake.nix`):** Provides a completely isolated and reproducible local build environment, managing dependencies like `west`, the Zephyr SDK, and Python.
- **`direnv` & `just`:** Automate the development environment setup and provide simple commands for common tasks like building the firmware (`just build`) and updating dependencies (`just update`).
- **GitHub Actions:** Automate the build process for continuous integration.
- **Keymap-drawer:** A tool for visualizing the keyboard layout.

## Key Design Patterns

*Document the primary design patterns in use and where they are applied.*

- **Layered Configuration via C Macros:** The keymap is built using a powerful C preprocessor macro pattern. `config/glove80.keymap` defines a `ZMK_BASE_LAYER` macro that establishes the static layout for the outer columns and thumb keys. The core logic in `config/base.keymap` then invokes this macro for each layer (Base, Nav, Fn, etc.), injecting the layer-specific bindings into the shared structure. This creates a highly modular and reusable layout system.
- **"Timeless" Homerow Mods:** A sophisticated implementation of homerow modifiers that avoids timing issues by using ZMK's `balanced` flavor, `require-prior-idle-ms`, and positional hold-tap features. This allows for a fluid typing experience with minimal misfires.
- **Combos Over Layers:** The configuration favors using key combos for symbols and common actions rather than dedicated layers. This reduces the need for lateral thumb movements and can be more ergonomic.
- **Smart Layers (Numword & Magic Shift):** The use of "smart" or auto-toggling layers like Numword (for typing numbers) and Magic Shift (which combines repeat, sticky-shift, and caps-word) to reduce cognitive load and improve efficiency.
- **Conditional Layers:** The `ZMK_CONDITIONAL_LAYER` macro is used to create layers that are activated only when multiple other layers are active (e.g., `FN + NUM -> SYS`).

## Data Flow

The configuration data flows from high-level, human-readable keymap files to the final compiled firmware:

1.  **`config/glove80.keymap`:** Defines the physical layout of the Glove80 and the `ZMK_BASE_LAYER` macro.
2.  **`config/base.keymap`:** Contains the core keymap logic, defining the different layers by invoking the `ZMK_BASE_LAYER` macro with specific keybindings. It also includes various `.dtsi` files for behaviors, homerow mods, combos, etc.
3.  **`build.yaml`:** Specifies the board and shield combinations to be built.
4.  **`west` & `just`:** Use this information to invoke the Zephyr/ZMK build system, which compiles the C/C++ and Devicetree source files into the final `.uf2` firmware file.
5.  **Firmware Flash:** The compiled firmware is then flashed onto the Glove80 keyboard.

## Error Handling Strategy

Given the complexity and interdependence of the keymap configuration, the primary error handling strategy is careful, iterative development and testing. When making changes, especially moving keys between the left and right splits, it is crucial to manually verify that all related modifiers, layers, and combos still function as expected. The visual representation from `keymap-drawer` can also serve as a tool for debugging and verification.
