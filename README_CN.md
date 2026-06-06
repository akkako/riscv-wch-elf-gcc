# Install WCH Modified GCC Toolchain Action

一个用于安装 WCH 修改版 RISC-V GCC 工具链并将其添加到系统 PATH 的 GitHub Composite Action。

## 功能特性

- **一键安装**：自动克隆工具链仓库、解压归档文件并配置环境。
- **智能缓存**：检测工具链是否已安装，避免重复操作。
- **PATH 自动注入**：通过 `$GITHUB_PATH` 将工具链暴露给工作流中的所有后续步骤。

## 使用方法

在你的工作流中添加以下步骤：

```yaml
- name: Install WCH modified GCC toolchain
  uses: ./.github/actions/install-wch-toolchain
```

## 示例工作流

```yaml
name: Build Firmware

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v5

      - name: Install WCH modified GCC toolchain
        uses: ./.github/actions/install-wch-toolchain

      - name: Build Application
        run: |
          cd firmware/Application
          make all -j
```

## 工作原理

1. **检查缓存**：确认 `~/riscv-wch-elf-gcc/Toolchain/gcc15/bin/riscv32-wch-elf-gcc` 是否已存在。
2. **执行安装（如需要）**：
   - 将 `https://github.com/akkako/riscv-wch-elf-gcc.git` 克隆到 `~/riscv-wch-elf-gcc`。
   - 解压 `MRS_Toolchain_Linux_x64_V240.tar.xz`。
   - 将 `RISC-V Embedded GCC15` 重命名为 `gcc15`，以保持路径一致。
3. **配置 PATH**：将工具链的 `bin` 目录追加到 `$GITHUB_PATH`。
4. **验证**：运行 `riscv32-wch-elf-gcc --version` 确认安装成功。

## 输入参数

本 Action 没有可配置的输入参数。

## 输出参数

本 Action 不产生输出参数。工具链通过修改运行器的 PATH 环境变量来提供使用。

## 环境要求

- `ubuntu-latest` 运行器（或任何具备 `bash`、`git` 和 `tar` 的 Linux 运行器）。

## 注意事项

- 工具链安装在运行器的家目录下（`~/riscv-wch-elf-gcc`）。
- 已安装的工具链仅在当前工作流作业（job）的持续时间内有效。
