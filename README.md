# Rubik's Cube Solver

A C++14 Rubik's Cube solver with multiple cube representations and search algorithms, plus a webcam-based scanner using OpenCV. The solver supports DFS, BFS, IDDFS, and IDA* (with a corner pattern database heuristic).

## Features
- **Cube models**: 3D array, 1D array, and bitboard (`Model/`)
- **Solvers**: DFS, BFS, IDDFS, IDA* (`Solver/`)
- **Heuristics**: Corner pattern database (`PatternDatabases/`)
- **Webcam scanner**: Capture cube colors and feed directly into the solver (`Scanner/`)

## Prerequisites
- CMake ≥ 3.20
- C++14 toolchain (e.g., clang or gcc)
- OpenCV (tested with 4.x)

On macOS (Homebrew):
```bash
brew install cmake opencv
```

Ensure CMake can find OpenCV (e.g., Homebrew sets `OpenCV_DIR` automatically). If not, export `OpenCV_DIR` to your OpenCV cmake config path.

## Build
```bash
mkdir -p build
cd build
cmake ..
cmake --build . --config Release
```
This produces the executable `rubiks_cube_solver`.

## Run
From the `build` directory:
```bash
./rubiks_cube_solver
```

By default, `main.cpp` runs the webcam scanner, constructs a `RubiksCubeBitboard`, and solves it with IDA* using the corner pattern database.

## Scanner usage
- The app opens your webcam (default camera index 0) and shows a 3×3 grid overlay.
- Align one cube face in the grid and press `SPACE` to capture.
- A preview window appears:
  - Press `N` to accept the face and proceed to the next.
  - Close-ups of all scanned faces are shown in a net layout for context.
- Repeat for all 6 faces. After scanning, the solver runs and prints the move sequence and final cube state in the console.

If you have multiple cameras, change the camera index passed to `CubeScanner` in `main.cpp` (constructor argument).

## Pattern database
An example corner database is included:
- `Databases/cornerDepth5V1.txt`

`main.cpp` expects a database file path string. Update it to your local path if needed. Example (in `main.cpp`):
```cpp
string fileName = "/absolute/path/to/Databases/cornerDepth5V1.txt";
```
Tip: prefer absolute paths to avoid working directory issues.

## Switching algorithms or models
`main.cpp` includes commented sections showing how to:
- Instantiate different cube models (`RubiksCube3dArray`, `RubiksCube1dArray`, `RubiksCubeBitboard`)
- Run DFS, BFS, IDDFS, or IDA*
Uncomment the relevant blocks and rebuild.

## Troubleshooting
- "Failed to open webcam": Ensure a working camera and adjust the camera index (e.g., `CubeScanner(1)`), and allow camera permissions.
- OpenCV not found: Verify OpenCV installation and that CMake locates it. Set `OpenCV_DIR` if needed, then re-run CMake from a clean `build/`.
- Blank or incorrect colors: Lighting affects color detection. Use diffuse lighting, avoid glare, and move the cube closer/farther so stickers fill each grid cell. You can tune HSV thresholds in `Scanner/CubeScanner.cpp` if required.
- File not found for database: Use an absolute path for the database file.

## Project structure
- `Model/`: Cube abstractions and implementations
- `Solver/`: Search algorithms
- `PatternDatabases/`: Pattern DB, nibble array, indexers, generators
- `Scanner/`: OpenCV-based color scanner
- `Databases/`: Precomputed databases
- `main.cpp`: Entry point showcasing scanning + solving
- `CMakeLists.txt`: Build configuration

## License
MIT (or project’s original license if different).