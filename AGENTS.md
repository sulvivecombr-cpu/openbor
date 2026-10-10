# OpenBOR — Base44 Environment Notes

## What this project is

OpenBOR is a **native C/SDL2 game engine** — a desktop application that compiles to a
native executable. It is **not a web application**: there is no web server, HTTP API,
or frontend. The Base44 preview (which expects a web server on port 3000) cannot
display a running game instance.

## Build

The project uses CMake 3.22+ and targets 64-bit architectures only.

```sh
# Linux AMD64 (verified working)
cmake -DBUILD_LINUX=ON -DTARGET_ARCH=AMD64 -DCMAKE_BUILD_TYPE=Release -S . -B build
cmake --build build -- -j$(nproc)
# Output: engine/releases/LINUX/OpenBOR
```

### Toolchain requirement

Release builds use `-Werror`, and `cmake/linux.cmake` passes `-Wno-void-pointer-to-enum-cast`
which requires **GCC 13+**. Debian bookworm (GCC 12) fails the Release build. Use
**Debian trixie** (GCC 14) or equivalent — this is what `docker-compose.base44.yml` uses.

Debug builds (`-DCMAKE_BUILD_TYPE=Debug`) do not use `-Werror` and compile on GCC 12.

### Dependencies (Linux)

```
cmake build-essential git libsdl2-dev libvorbis-dev libpng-dev libvpx-dev
```

### Runtime

The binary requires SDL2, a display, and audio output. It cannot run headless or in
a browser. Run it on a Linux desktop with the SDL2 runtime libraries installed.

## Cross-compilation

The repo's `.devcontainer/Dockerfile` provides toolchains for Linux AMD64, Linux ARM64,
and Windows AMD64 (MinGW). The aggregate script `build-all.sh` builds all three.

## No external secrets

The project has no external service dependencies. No credentials are needed.
