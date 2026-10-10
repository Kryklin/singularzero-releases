<div align="center">

  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/header-anim-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="assets/header-anim-light.svg">
    <img alt="SingularZero Banner" src="assets/header-anim-dark.svg" width="100%">
  </picture>

  <br><br>

  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/logo-anim-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="assets/logo-anim-light.svg">
    <img alt="SingularZero Logo" src="assets/logo-anim-dark.svg" width="200" height="200">
  </picture>

  <br>

  <p>
    <img src="https://img.shields.io/badge/version-0.1.0-blue.svg" alt="Version">
    <img src="https://img.shields.io/badge/license-MIT-green.svg" alt="License">
    <img src="https://img.shields.io/badge/protocol-UCI-orange.svg" alt="Protocol">
    <img src="https://img.shields.io/badge/platform-Windows%20%7C%20Linux-lightgrey.svg" alt="Platform">
    <img src="https://img.shields.io/badge/SIMD-AVX--512%20%7C%20AVX2%20%7C%20Scalar%20Auto--Dispatch-brightgreen.svg" alt="Architecture">
  </p>

  <p>
    <strong>Advanced, High-Performance Autonomous Chess Engine</strong><br>
    <em>Developed by <strong>Ellie Slight</strong> • Written in pure systems Rust • Zero third-party dependencies</em>
  </p>

</div>

---

## Unified Binary (Automatic Hardware Dispatch)

SingularZero distributes **one single executable** per operating system. You do not need to choose between multiple instruction set builds:

* **Windows:** [`singularzero.exe`](https://github.com/Kryklin/singularzero-releases/releases/latest/download/singularzero.exe)
* **Linux:** [`singularzero-linux`](https://github.com/Kryklin/singularzero-releases/releases/latest/download/singularzero-linux)

### How Hardware Detection Works at Runtime
Upon launching, the engine queries CPUID capabilities once via lockless atomic probes:
1. **AVX-512:** If 512-bit vector extensions (`avx512f`) are available, the engine executes through 512-bit ZMM vector pipelines.
2. **AVX2 + FMA:** If 256-bit vector extensions (`avx2`, `fma`) are present, it executes 8-wide SIMD instructions.
3. **Scalar Fallback:** If run on legacy hardware or virtual machines lacking AVX, it automatically falls back to scalar execution with zero configuration.

---

## Key Features

* **Bitboard Representation:** High-speed 64-bit bitboards with magic bitboard multipliers for ultra-fast sliding piece move generation.
* **Multi-Threaded Negamax Search:** Principal Variation Search (PVS), Staged Move Picking, Quiescence Search with Stand-Pat pruning, Late Move Reductions (LMR), Null Move Pruning, and Adaptive History Heuristics.
* **Hybrid Dynamic Evaluation:** Blends handcrafted positional features (PSQT, lockless pawn structure tables, rook file penetration, bishop pair bonuses, king safety geometry) with high-dimensional tactical scoring.
* **Lockless Transposition Table:** Clustered, cacheline-aligned transposition table with age tracking, depth replacement, and thread-safe lockless probing.
* **Endgame Tablebase Support:** Native Syzygy 3-4-5-6 piece endgame tablebase probing via `SyzygyPath`.
* **Polyglot Opening Book:** Built-in opening book probing with configurable book file loading.

---

## GUI Installation

SingularZero is a headless chess engine designed to be loaded into any standard chess GUI.

### 1. Cutechess / Cutechess-cli
1. Go to **Tools -> Settings -> Engines -> Add...**
2. Browse to the downloaded `singularzero.exe` (or `singularzero-linux`).
3. Select protocol: **UCI**.
4. Set memory (`Hash`) and core count (`Threads`) according to your hardware.

### 2. Arena Chess GUI
1. Open **Engines -> Manage -> Details -> New**.
2. Select `singularzero.exe`.
3. Choose **UCI** protocol and click **Apply**.

### 3. Banksia GUI / Fritz / ChessBase / Nibbler
1. Add new UCI engine and point to the binary path.
2. The engine will automatically register and initialize.

---

## Interactive Command-Line Mode

If executed directly in a terminal or console, SingularZero launches an interactive diagnostic environment:

```text
  ===============================================================
                 SINGULARZERO CHESS ENGINE v0.1.0                
               High-Performance Modern Chess Engine             
  ===============================================================

  [*] 64-bit Bitboard Engine & Magic Bitboard Multipliers
  [*] Multi-Threaded Negamax Alpha-Beta with PVS & Quiescence
  [*] Hybrid Positional & Dynamic Tactical Evaluation
  [*] Lockless High-Throughput Transposition Table Cluster
  [*] Universal Chess Interface (UCI) Protocol Driver

Type 'help' for commands or 'play' to challenge the engine.

singularzero> 
```

### Available Console Commands
* `uci`: Switch to standard UCI protocol mode.
* `play [white|black]`: Start an interactive match against SingularZero.
* `move <e2e4>`: Input a move in algebraic coordinate format.
* `go`: Instruct the engine to compute and execute the best move from the current position.
* `eval`: Display a static positional and material evaluation breakdown.
* `d` / `display`: Render the current 8x8 ASCII board.
* `perft <depth>`: Run move generator perft validation and measure raw node throughput.
* `bench [threads]`: Run the built-in tactical and strategic crucible test suite.
* `fen <fen string>`: Set the board to a specific FEN position.
* `undo`: Undo the previous half-move.
* `new`: Reset to the standard starting position.
* `quit`: Exit the engine.

---

## Configurable UCI Options

| Option Name | Type | Default | Range | Description |
| :--- | :--- | :--- | :--- | :--- |
| **`Hash`** | `spin` | *Auto-detected* | `1` – `65536` | Memory allocated to the Transposition Table in Megabytes (MB). |
| **`Threads`** | `spin` | *Auto-detected* | `1` – `512` | Number of CPU search worker threads. |
| **`Mode`** | `combo` | `Cascade` | `Cascade`, `Hybrid`, `Fast`, `Direct` | Engine tactical decision mode (`Cascade` recommended). |
| **`SyzygyPath`** | `string` | `<empty>` | Any valid path | Directory containing `.rtm.bin` and `.rtbw.bin` Syzygy tablebases. |
| **`OwnBook`** | `check` | `true` | `true` / `false` | Enable or disable opening book probing. |
| **`BookFile`** | `string` | `book.bin` | Any `.bin` file | Polyglot opening book file path. |

---

## Verifying Integrity

Each official release asset includes a `SHA256SUMS.txt` file. You can verify your download with:

```powershell
# Windows PowerShell
Get-FileHash .\singularzero.exe -Algorithm SHA256
```

```bash
# Linux
sha256sum -c SHA256SUMS.txt
```

---

## Author & License

* **Author:** Ellie Slight
* **License:** [MIT License](LICENSE)
