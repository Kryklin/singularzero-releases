# SingularZero — Configuration & Usage Guide

Comprehensive reference guide for configuring **SingularZero**, connecting it to chess GUIs, utilizing command-line launch options, and navigating the interactive console.

---

## 1. GUI Installation & Setup

SingularZero is a headless engine compliant with the **Universal Chess Interface (UCI)** protocol. It is compatible with all major chess GUIs, tournament managers, and online bot frameworks.

### Cutechess / Cutechess-cli
1. Open **Tools -> Settings -> Engines -> Add...**
2. Browse to `singularzero.exe` (or `singularzero-linux`).
3. Select protocol: **UCI**.
4. Set memory (**Hash**) and core count (**Threads**) to match your hardware.
5. Click **OK** to save.

### Arena Chess GUI
1. Open **Engines -> Manage -> Details -> New**.
2. Select the `singularzero.exe` executable.
3. Select **UCI** protocol and click **Apply**.

### Banksia GUI / Fritz / ChessBase / Nibbler
1. Open your GUI's engine settings and add a new UCI engine.
2. Select the binary path. The engine will automatically register options and capabilities.

---

## 2. Command-Line Launch Options

You can launch SingularZero from your terminal with various operational flags:

| Flag | Argument | Description |
| :--- | :--- | :--- |
| *(no arguments)* | — | Standard auto-dispatch: Runs in **UCI protocol mode** when called by a GUI/pipe, or launches the **Interactive Console** in a direct terminal. |
| `--uci` | — | Forces standard headless UCI protocol mode regardless of terminal state. |
| `--cli` | — | Forces the interactive terminal chess console with ASCII board rendering. |
| `--bench` | `[threads]` | Runs the embedded tactical & strategic benchmark suite and reports total NPS and time. |
| `--match` | `[games] [depth]` | Runs an automated internal self-play match and outputs win/loss/draw statistics. |
| `--tune` | `[iter] [games] [depth]` | Runs automated SPSA parameter self-play optimization. |

---

## 3. Configurable UCI Options

When connected to any chess GUI, SingularZero exposes standard parameters configurable via the GUI settings dialog:

| Option Name | Type | Default | Range | Description |
| :--- | :--- | :--- | :--- | :--- |
| **`Hash`** | `spin` | *Auto-detected* | `1` – `65536` | Memory allocated to the Transposition Table in Megabytes (MB). Default auto-sizes based on available physical RAM. |
| **`Threads`** | `spin` | *Auto-detected* | `1` – `512` | Number of worker threads utilized during search. Default matches available CPU hardware threads. |
| **`Mode`** | `combo` | `Cascade` | `Cascade`, `Hybrid`, `Fast`, `Direct` | Engine decision pipeline (`Cascade` provides peak Grandmaster strength). |
| **`SyzygyPath`** | `string` | `<empty>` | Any valid path | Directory containing `.rtm.bin` and `.rtbw.bin` Syzygy endgame tablebases. Probes up to 6-piece endgames. |
| **`OwnBook`** | `check` | `true` | `true` / `false` | Enable or disable instant opening book lookups. |
| **`BookFile`** | `string` | `book.bin` | Any `.bin` file | Path to a custom Polyglot opening book file. |

---

## 4. Interactive Console Mode Commands

When launched in terminal mode (`--cli` or direct console execution), SingularZero provides an interactive command shell:

```text
  ===============================================================
                 SINGULARZERO CHESS ENGINE v0.1.0                
               High-Performance Modern Chess Engine             
  ===============================================================

Type 'help' for commands or 'play' to challenge the engine.
singularzero> 
```

### Available Console Commands

* **`play [white|black]`**: Start an interactive match against SingularZero. The engine immediately begins playing.
* **`move <e2e4>`** / **`<e2e4>`**: Input a move in standard coordinate notation.
* **`go`**: Instruct the engine to search the current position and make the best move.
* **`eval`**: Display a static positional breakdown (PSQT balance, pawn structure score, side to move).
* **`d`** / **`display`**: Render the current 8×8 board state in the terminal.
* **`undo`** / **`u`**: Take back the previous half-move.
* **`new`** / **`reset`**: Reset the board to the standard starting position.
* **`fen <fen string>`**: Load a custom position from a FEN string.
* **`perft <depth>`**: Run move generator validation and measure raw move generation throughput (NPS).
* **`bench [threads]`**: Run the built-in tactical and strategic benchmark suite.
* **`uci`**: Switch immediately into headless UCI protocol driver mode.
* **`quit`** / **`exit`**: Exit the engine.
