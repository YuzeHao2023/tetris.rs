<img src="img/tetris.gif" align="right">

# Tetris.rs — 俄罗斯方块（Piston + Rust）  
A small Tetris clone written in Rust using piston_window.  
一个用 Rust 和 piston_window 实现的简易俄罗斯方块游戏。

---

## 目录 / Contents
- [简介 / Summary](#简介--summary)  
- [特色 / Features](#特色--features)  
- [要求 / Requirements](#要求--requirements)  
- [快速开始 / Quick Start](#快速开始--quick-start)  
- [运行与控制 / Controls](#运行与控制--controls)  
- [项目结构 / Project Structure](#项目结构--project-structure)  
- [实现细节 / Implementation Details](#实现细节--implementation-details)  
- [自定义 / Customization](#自定义--customization)  
- [常见问题 / FAQ & Troubleshooting](#常见问题--faq--troubleshooting)  
- [贡献 / Contributing](#贡献--contributing)  
- [许可 / License](#许可--license)

---

## 简介 / Summary
这是一个用 Rust 编写、基于 piston_window 的俄罗斯方块实现。代码简洁、依赖少，适合作为学习 Rust 图形应用或小型游戏工程的示例。  
This is a Tetris clone written in Rust using the piston_window crate. The codebase is small and dependency-light, making it a good example for learning Rust GUI/game programming and simple game architecture.

---

## 特色 / Features
- 用纯 Rust 编写（依赖 piston_window 和 rand）。  
- 简单的方块渲染、旋转、下落和消行特效（闪烁）。  
- 简洁的 Board 数据结构，支持变换（旋转、镜像、位移）与合并检测。  
- 可复用的渲染与游戏状态机（Falling / Flashing / GameOver）。  
- 支持键盘控制与重启。  

- Written entirely in Rust (depends on piston_window and rand).  
- Simple block rendering, rotation, falling, and line-clear visual effects (flashing).  
- A compact Board data structure supporting transforms (rotate, mirror, translate) and merge/collision checks.  
- Reusable rendering and a small game state machine (Falling / Flashing / GameOver).  
- Keyboard controls and restart support.

---

## 要求 / Requirements
- Rust (推荐 stable 或更高)  
  - 安装： https://rustup.rs  
- Cargo（随 rustup 提供）  
- piston_window 与 rand 作为 crate 依赖（已在 Cargo.toml 中指定）  
- 在某些系统上运行图形窗口需要系统级库（例如 X11 / Wayland / macOS 的图形库）。如果遇到窗口创建或依赖构建错误，请参考 piston_window 文档或相关系统包管理器安装指引。

- Rust (stable or newer recommended)  
  - Install via: https://rustup.rs  
- Cargo (bundled with rustup)  
- The project depends on piston_window and rand (declared in Cargo.toml)  
- On some systems you may need system-level graphics libraries (for example, X11/Wayland, OpenGL, or macOS graphics frameworks). If you run into window creation or build/link errors, consult piston_window's documentation or your platform's package manager for required development libraries.

---

## 快速开始 / Quick Start

1. 克隆仓库 / Clone:
```bash
git clone https://github.com/dvlan26/tetris.rs.git
cd tetris.rs
```

2. 使用 Rust toolchain 构建并运行 / Build and run:
```bash
# 开发模式（较慢但支持调试信息）
cargo run

# 或者发布模式（更快）
cargo run --release
```

3. 如果遇到链接或构建错误，请确保系统安装了基础 X/Window/GL 依赖。可查看 piston_window 的 README 以获取平台特定依赖信息。  
If you encounter linking or build errors, ensure your system has the required X/Window/OpenGL libraries installed. Refer to the piston_window README for platform-specific dependency instructions.

---

## 运行与控制 / Controls
- 左箭头 / Left: 向左移动方块 (Key::Left)  
- 右箭头 / Right: 向右移动方块 (Key::Right)  
- 下箭头 / Down: 向下加速 (Key::Down)  
- 上箭头 / Up: 顺时针旋转 (Key::Up)  
- 小键盘 5 / Numpad5: 逆时针旋转 (Key::NumPad5)  
- 回车 / Enter: 在 Game Over 时重启游戏 (Key::Return)  
- ESC: 退出窗口（由 piston_window 的 exit_on_esc 处理）

说明：键映射使用 piston_window 的 Button/Key 枚举。  
- Left Arrow: move the piece left (Key::Left)  
- Right Arrow: move the piece right (Key::Right)  
- Down Arrow: soft drop / accelerate down (Key::Down)  
- Up Arrow: rotate clockwise (Key::Up)  
- Numpad 5: rotate counter-clockwise (Key::NumPad5)  
- Enter: restart the game when Game Over (Key::Return)  
- ESC: exit the window (handled by piston_window's exit_on_esc)

Note: key mappings use piston_window's Button/Key enums.

---

## 项目结构 / Project Structure
- main.rs — 程序入口与核心实现（渲染、事件循环、游戏逻辑）。  
- Cargo.toml — 依赖声明（piston_window、rand 等）。  
- img/ — 存放 README 演示 gif 的目录（请将 demo gif 放入该目录）。  
- 其它资源/配置（如果有）将在仓库中体现。  

核心类型与作用（high-level）:
- Board — 一个包含方块坐标与颜色的容器（内部为 HashMap<(i8,i8), Color>）。  
- Metrics — 显示与方块大小配置（board_x, board_y, block_pixels）。  
- State — 游戏状态机: Falling(Board), Flashing(stage, last_switch, lines), GameOver。  
- Game — 核心逻辑：管理 board、当前下落图形、旋转、移动、消行与渲染。

- main.rs — program entrypoint and core implementation (rendering, event loop, game logic).  
- Cargo.toml — dependency declarations (piston_window, rand, etc.).  
- img/ — directory for README demo gif (put the demo gif here).  
- Other resources/configs (if any) are present in the repository.

Core types and roles (high-level):
- Board — container for block coordinates and colors (internally a HashMap<(i8,i8), Color>).  
- Metrics — display and block size configuration (board_x, board_y, block_pixels).  
- State — the game state machine: Falling(Board), Flashing(stage, last_switch, lines), GameOver.  
- Game — core logic: manages the board, current falling piece, rotations, movement, line clears, and rendering.

---

## 实现细节 / Implementation Details

- Board 的变换方法：
  - modified / modified_filter: 根据映射函数移动或过滤坐标。  
  - transposed / mirrored_y / rotated / rotated_counter: 用于旋转和镜像方块矩阵。  
  - negative_shift / shifted: 用于规范化方块坐标（去负坐标）并放置到目标位置。  
  - merged: 合并两个 Board（若存在重叠则返回 None）。  
  - contained: 检查 Board 的所有方块是否都在给定宽高范围内。  
  - whole_lines: 计算被填满的整行索引。  
  - kill_line: 移除某一行并下移上方方块（用于消行）。

- Game 的主要流程：
  - new_falling: 随机挑选下一个方块并随机旋转若干次，同时在放置前进行合法性检查（如果无法合并，触发 GameOver）。  
  - progress: 每隔一段时间推进方块下落，处理闪烁动画与实际的行移除（Flashing 状态）等。  
  - move_falling: 处理移动逻辑（检测碰撞、边界）并在无法下落时将方块固定到 board 并触发消行检测。  
  - rotate: 通过旋转矩阵、消除负位移并尝试不同挂墙偏移以实现基础的“墙蹭”（kick）处理。  

示例：在 main.rs 中可以看到创建 pieces 的代码：
```rust
self.possible_pieces: vec![
    Board::new(&[(0, 0), (0, 1), (1, 0), (1, 1), ][..], Color::Red), // O
    Board::new(&[(0, 0), (1, 0), (1, 1), (2, 0), ][..], Color::Green), // T-like
    Board::new(&[(0, 0), (1, 0), (2, 0), (3, 0), ][..], Color::Blue), // I
    ...
]
```

- Board transformation utilities:
  - modified / modified_filter: move or filter coordinates according to a mapping function.  
  - transposed / mirrored_y / rotated / rotated_counter: used to rotate and mirror piece matrices.  
  - negative_shift / shifted: normalize block coordinates (remove negative positions) and place them at a target offset.  
  - merged: merge two Boards (returns None if overlap occurs).  
  - contained: check whether all blocks of a Board are within given width/height bounds.  
  - whole_lines: compute indices of fully occupied rows.  
  - kill_line: remove a row and shift blocks above down (used for clearing lines).

- Main Game flow:
  - new_falling: choose a next piece at random and apply random rotations; check placement legality before spawning (if it cannot be merged, trigger GameOver).  
  - progress: advance falling by elapsed time, handle flashing animations, and perform actual line removals when flashing finishes.  
  - move_falling: handle movement logic (collision and boundary checks); when the piece cannot fall further, lock it into the board and check for completed lines.  
  - rotate: rotate using matrix transforms, normalize negative shifts, and try several wall-kick offsets to allow basic wall kicks.

---

## 自定义 / Customization
- 更改画面大小：在 main.rs 的 Metrics 配置里修改 `block_pixels`, `board_x`, `board_y`。  
- 添加新方块：向 `possible_pieces` 向量中添加新的 Board 生成器（注意坐标与颜色）。  
- 调整下落速度：在 Game::progress 中修改 `Duration::from_millis(700)` 的值以改变自然下落间隔。  
- 改变颜色或渲染风格：编辑 Board::render 中的颜色数组或边框计算逻辑。  

示例：将下落时间缩短为 400ms（更快）
```rust
if self.time_since_fall.elapsed() <= Duration::from_millis(400) {
    return;
}
```

- Change screen/board size by editing Metrics (`block_pixels`, `board_x`, `board_y`) in main.rs.  
- Add new pieces by appending new Board generators to the `possible_pieces` vector (pay attention to coordinates and color).  
- Adjust fall speed by changing `Duration::from_millis(700)` inside Game::progress.  
- Modify colors or rendering style by editing the color array or border logic in Board::render.

Example: shorten fall interval to 400ms for a faster game:
```rust
if self.time_since_fall.elapsed() <= Duration::from_millis(400) {
    return;
}
```

---

## 常见问题 / FAQ & Troubleshooting

- 窗口无法打开或编译失败：
  - 确保安装了 Rust toolchain（rustup、cargo）。  
  - 在 Linux 上，若出现链接错误，可能需要安装系统级的图形依赖（如 X11 开发库、OpenGL、libxcb 等），具体依赖请参照 piston_window 的官方说明。  
- 游戏一开始直接 GameOver：
  - 这意味着新生成的方块在初始位置与现有固定方块冲突。清除 board 或重启游戏可以恢复；也可能需要调整 `new_falling` 中的 spawn 位置或 `shift` 初始值以避免冲突。  
- 如何调试或打印信息？
  - 在适当位置加入 println!() 调试输出，或在 IDE 中使用断点调试（例如使用 CLion / VSCode + rust-analyzer）。  
- 运行卡顿或帧率低：
  - 在 release 模式下构建（cargo run --release）通常会明显提升性能。也可检查绘制逻辑是否有不必要的分配或复杂计算。

- Window won't open or compilation fails:
  - Make sure Rust toolchain is installed (rustup, cargo).  
  - On Linux, linking errors often indicate missing system graphics/development libraries (X11, OpenGL, libxcb, etc.). See piston_window's documentation for platform-specific dependencies.  
- Game immediately shows GameOver on start:
  - This means the newly spawned piece collides with existing locked blocks at spawn position. Clearing the board or restarting will fix it; otherwise consider adjusting spawn offset or initial shift in `new_falling`.  
- How to debug or print information:
  - Insert println!() at relevant places or use an IDE debugger (CLion, VSCode + rust-analyzer).  
- Poor performance or low FPS:
  - Building in release mode (cargo run --release) usually improves performance significantly. Also check rendering paths for unnecessary allocations or heavy computations.

---

## 贡献 / Contributing
欢迎贡献。常见方式：
- Fork 本仓库并创建 feature 分支。  
- 提交清晰的 commit，写明改动原因。  
- 发起 Pull Request 并在 PR 中说明测试方法与预期行为。  

贡献建议：
- 保持代码风格一致（遵循 rustfmt）  
- 写明平台与测试步骤（macOS / Linux / Windows）  
- 对大型改动请先创建 Issue 讨论设计

Contributions are welcome. Typical workflow:
- Fork the repository and create a feature branch.  
- Make clear commits describing the changes.  
- Open a Pull Request describing how to test and the expected behavior.

Contribution tips:
- Keep code style consistent (run rustfmt).  
- State platform and testing steps (macOS / Linux / Windows).  
- For large design changes, open an Issue first to discuss.

---

## 许可 / License
请查阅仓库中的 LICENSE 文件以获知具体许可信息。  
See the LICENSE file in this repository for license details.

---

## 贡献者 / Contributors

<div align="center">
  <h4 align="center">
    Contributors are the cornerstone of the project.
  </h4>
  <a href="https://github.com/YuzeHao2023" target="_blank">
    <img src="https://github.com/YuzeHao2023.png" width="100" height="100" style="border-radius: 50%; margin: 0 10px; object-fit: cover;" alt="YuzeHao2023" />
  </a>
  <a href="https://github.com/dvlan26" target="_blank">
    <img src="https://github.com/dvlan26.png" width="100" height="100" style="border-radius: 50%; margin: 0 10px; object-fit: cover;" alt="dvlan26" />
  </a>  
</div>

