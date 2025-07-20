# Technical Context

This document outlines the technologies, tools, and technical environment for the project.

## Core Technologies

- **Firmware:** ZMK Firmware
- **Languages:** C, C++
- **Configuration:** Devicetree (`.dtsi`, `.keymap` files)
- **Build System:** Zephyr RTOS build system

## Development Environment

The development environment is fully managed and reproducible using Nix.

- **Package Manager:** Nix is used to manage all system-level dependencies.
- **Environment Automation:** `direnv` is used to automatically load the Nix shell when entering the project directory.
- **Task Runner:** `just` provides simple, consistent commands for common development tasks (e.g., `just build`, `just update`).
- **Core Dependencies (from `flake.nix`):**
  - `nixpkgs`: The source for all Nix packages.
  - `zephyr-nix`: Provides the Zephyr SDK and Python environment.
  - `cmake`, `dtc`, `ninja`: Core build tools for Zephyr/ZMK.
  - `yq`: A command-line YAML processor.
  - `git`: For version control.

## ZMK Modules & Dependencies

The `west` tool manages the ZMK firmware and its modules, with versions pinned in `config/west.yml`.

- **Core Dependency:** `zmkfirmware/zmk`
- **Custom Modules (from `urob`):**
  - `zmk-adaptive-key`
  - `zmk-auto-layer`
  - `zmk-helpers`
  - `zmk-leader-key`
  - `zmk-tri-state`
- **Zephyr Hardware Abstraction Layers (HALs):**
  - `cmsis`
  - `hal_nordic`
  - `hal_rpi_pico`
  - `hal_stm32`
- **Other Libraries:**
  - `lvgl`: Light and Versatile Graphics Library.
  - `tinycrypt`: Cryptography library.

## Continuous Integration & Visualization

- **Visualization:** `keymap-drawer` is used to generate visual representations of the keymap from the configuration files.
- **Continuous Integration (GitHub Actions):** The project uses two distinct GitHub Actions workflows for CI/CD:
  - **`build.yml`:** Triggers a standard build using the official `zmkfirmware/zmk` reusable workflow. This provides a baseline build process maintained by the ZMK community.
  - **`build-nix.yml`:** Triggers a Nix-based build using a reusable workflow from `urob/zmk-actions`. This workflow mirrors the local development environment, ensuring high fidelity between local and CI builds.

## Technical Constraints

- **Pinned Versions:** All dependencies, including ZMK and its modules, are pinned to specific revisions in `config/west.yml` and `flake.lock`. Upgrading requires careful testing.
- **Nix Environment:** The build process is dependent on the Nix ecosystem. Development outside of this environment is not supported.
