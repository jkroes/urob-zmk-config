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

## Build Pathways

The project has two primary pathways for building the firmware: a local, Nix-based environment and an automated CI/CD pipeline using GitHub Actions.

### 1. Local Build Pathway (Nix & `just`)

This pathway provides a fully reproducible and automated local development environment.

1.  **Environment Activation:** Upon entering the project directory, `direnv` uses `flake.nix` to activate a Nix shell. This shell provides all necessary, version-pinned dependencies, including the Zephyr SDK, `west`, `python`, `just`, and `yq`.
2.  **Build Initiation:** The developer runs the `just build` command.
3.  **Task Execution (`Justfile`):**
    *   **Dynamic Config:** The `_parse_combos` recipe runs first, analyzing `config/combos.dtsi` and dynamically updating the `CONFIG_ZMK_COMBO_MAX_COMBOS_PER_KEY` and `CONFIG_ZMK_COMBO_MAX_KEYS_PER_COMBO` settings in the `.conf` files.
    *   **Target Parsing:** The `_parse_targets` recipe uses `yq` to read `build.yaml` and generate a list of build targets (e.g., `glove80_lh` and `glove80_rh`).
    *   **Firmware Compilation:** The `_build_single` recipe iterates through each target and executes the core `west build` command.
4.  **Core Build (`west`):**
    *   The `west build` command is invoked with the correct board, shield, and the `-DZMK_CONFIG` flag pointing to the `config/` directory.
    *   `west` uses the `config/west.yml` manifest to assemble the full source tree, including the correct versions of ZMK, Zephyr, and all custom modules.
5.  **Output:** The final `.uf2` firmware files are copied from the `.build/` directory into the `firmware/` directory.

### 2. GitHub Actions Pathways

The project uses two distinct, reusable GitHub Actions workflows for automated builds.

*   **Pathway A: Standard ZMK Build (`.github/workflows/build.yml`)**
    *   This workflow uses the official `zmkfirmware/zmk/.github/workflows/build-user-config.yml@main` reusable workflow.
    *   It provides a standard, non-Nix build environment maintained by the ZMK project. It reads `build.yaml` to generate a build matrix and uploads the compiled firmware as a build artifact.
*   **Pathway B: Nix-Based ZMK Build (`.github/workflows/build-nix.yml`)**
    *   This workflow uses a specialized, Nix-based reusable workflow from `urob/zmk-actions/.github/workflows/build-user-config.yml@main`.
    *   This method mirrors the local development environment by setting up Nix on the GitHub runner, installing dependencies from `flake.nix`, and then running the build. This ensures maximum consistency between local and CI builds.

## Data Flow

The configuration data flows from high-level, human-readable keymap files to the final compiled firmware, orchestrated by the build pathways described above.

1.  **`config/glove80.keymap`:** Defines the physical layout of the Glove80 and the `ZMK_BASE_LAYER` macro.
2.  **`config/base.keymap`:** Contains the core keymap logic, defining the different layers by invoking the `ZMK_BASE_LAYER` macro with specific keybindings. It also includes various `.dtsi` files for behaviors, homerow mods, combos, etc.
3.  **`build.yaml`:** Specifies the board and shield combinations to be built, consumed by both local and CI build processes.
4.  **Build Execution:** The selected build pathway (`just` or GitHub Actions) invokes the Zephyr/ZMK build system.
5.  **Firmware Output:** The build system compiles the C/C++ and Devicetree source files into the final `.uf2` firmware file.

## Error Handling Strategy

Given the complexity and interdependence of the keymap configuration, the primary error handling strategy is careful, iterative development and testing. When making changes, especially moving keys between the left and right splits, it is crucial to manually verify that all related modifiers, layers, and combos still function as expected. The visual representation from `keymap-drawer` can also serve as a tool for debugging and verification.
