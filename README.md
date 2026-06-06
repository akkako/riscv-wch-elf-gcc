

[中文](README_CN.md)

# Install WCH Modified GCC Toolchain Action

A GitHub Composite Action to install the WCH modified RISC-V GCC toolchain and add it to the system PATH.

The name of gcc toolchain is `riscv32-wch-elf-gcc`, the version is `15.2.0`.

## Features

- **One-step installation**: Automatically clones the toolchain repository, extracts the archive, and configures the environment.
- **Smart caching**: Detects if the toolchain is already installed and skips redundant steps.
- **PATH auto-injection**: Uses `$GITHUB_PATH` to make the toolchain available to all subsequent steps in the workflow.

## Usage

Add the following step to your workflow:

```yaml
- name: Install WCH modified GCC toolchain
  uses: akkako/riscv-wch-elf-gcc@v1
```

## Example Workflow

```yaml
name: Build Firmware

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v5

      - name: Install WCH modified GCC toolchain
        uses: akkako/riscv-wch-elf-gcc@v1

      - name: Build Application
        run: |
          cd firmware/Application
          make all -j
```

## How It Works

1. **Check Cache**: Verifies if `~/riscv-wch-elf-gcc/Toolchain/gcc15/bin/riscv32-wch-elf-gcc` already exists.
2. **Install (if needed)**:
   - Clones `https://github.com/akkako/riscv-wch-elf-gcc.git` to `~/riscv-wch-elf-gcc`.
   - Extracts `MRS_Toolchain_Linux_x64_V240.tar.xz`.
   - Renames `RISC-V Embedded GCC15` to `gcc15` for a consistent path.
3. **Configure PATH**: Appends the toolchain `bin` directory to `$GITHUB_PATH`.
4. **Verify**: Runs `riscv32-wch-elf-gcc --version` to confirm the installation.

## Inputs

This action has no configurable inputs.

## Outputs

This action produces no outputs. The toolchain is made available by modifying the runner's PATH.

## Requirements

- `ubuntu-latest` runner (or any Linux-based runner with `bash`, `git`, and `tar` available).

## Notes

- The toolchain is installed to the runner's home directory (`~/riscv-wch-elf-gcc`).
- The installed toolchain persists only for the duration of the workflow job.
