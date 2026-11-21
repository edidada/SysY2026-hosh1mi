# SysY2026 编译器（Hosh1mi）— 中文说明

> 上游：`Hosh1mi/SysY2026Compiler`，2026 年全国大学生计算机系统能力大赛「华为毕昇杯」编译系统设计赛 **ARM 后端赛道国一等奖**作品。本仓库为其镜像，仅新增 CI workflow 与中文说明。

## 版本对齐 SysY2026

- 仓库内自带 `docs/SysY2026-def.pdf`（2026 语言规范）及 `docs/SysY2022-Language-Definition-V1-English.pdf`、`SysY2022-Runtime-Library-V1-English.pdf`——即 **SysY2026 以 SysY2022 为基础**。
- 目标平台：**ARMv8-A 64 位，Cortex-A53（NEON、FP）**，对应技术方案第 23 条中 int/float 与向量类型支持的硬件。
- 编译器接口：`compiler -S -o out.s <.sy> [-O1]`；`lib/libsysy.a` 为运行时库。

## 构建方法

纯 C++17 + bison/flex，不依赖 LLVM。

```bash
sudo apt install -y build-essential cmake ninja-build bison flex \
  gcc-riscv64-linux-gnu g++-riscv64-linux-gnu
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build -j4

# 冒烟测试
echo 'int main() { return 0; }' > /tmp/smoke.sy
./build/compiler -S -o /tmp/smoke.s /tmp/smoke.sy
```

## CI 状态

- GitHub Actions：`.github/workflows/build.yml`，安装依赖 → CMake 构建 → 生成汇编冒烟测试。
- 状态：✅ 通过（run `44903402936`，约 1m13s）。
- Gitee 镜像：`https://gitee.com/edidada/SysY2026-hosh1mi`（`main`）。
- 本地：未编译，全部交给 CI。
- 注：上游 `.gitignore` 首行 `/*` 忽略所有顶层文件，已加 `!.github/` 例外，CI 才能提交。
