# llvm-builds

Prebuilt LLVM for [grimace](https://github.com/grimace-sdk/grimace): clang, lld and the LLVM tools with the SuperH (SH-3, Windows CE), ARM (Windows CE) and MIPS targets, from the `wince-arm` branch of [grimace-sdk/llvm-project](https://github.com/grimace-sdk/llvm-project/tree/wince-arm), which builds on `wince-sh3`. Its `armv4-unknown-none-wince` target follows Microsoft's Windows CE ARM calling standard, which no LLVM release has.

Downloads are on the [releases](../../releases) page, for macOS (Apple Silicon) and Linux (x86_64). Releases up to v3 are named `velo-llvm-*`. Unpack one and set `GRIMACE_LLVM_ROOT` to it:

```sh
tar -xJf velo-llvm-v3-macos-arm64.tar.xz
cmake -S . -B build -DCMAKE_TOOLCHAIN_FILE=/path/to/grimace/cmake/toolchain.cmake -DGRIMACE_ARCH=sh3 -DGRIMACE_LLVM_ROOT=$PWD/velo-llvm-v3-macos-arm64
```

MIPS and x86 don't need this: grimace works with Homebrew's or the distribution's LLVM for those.

## What's in it

`clang`, `ld.lld`, `llvm-ar`, `llvm-ranlib`, `llvm-rc`, `llvm-objcopy`, `llvm-strip`, `llvm-objdump`, `llvm-readobj`, `llvm-readelf`, `llvm-nm`, `llvm-size`, `llvm-symbolizer`, `llvm-addr2line`, `llvm-mc`, and clang's headers. From v3, also libc++'s headers (`include/c++/v1`) and sources (`share/libcxx/src`), which grimace uses for C++ standard library support with its own configuration. Release build with assertions on, since the SuperH backend is experimental and the Windows CE ARM ABI is new: a compiler bug stops with an error instead of producing bad code.

## Building a release

`LLVM_REF` holds the llvm-project commit a release is built from. To release, update it, commit, and push a `v*` tag; the workflow builds both platforms and publishes them. Run the workflow by hand (Actions > Build > Run workflow) to build any ref as artifacts without releasing.

The scripts in `scripts/` also work locally, given an llvm-project checkout:

```sh
scripts/configure.sh ../llvm-project build
scripts/build.sh build
scripts/package.sh build ../llvm-project llvm-local dist
```

## Licence

The scripts here are MIT (`LICENSE`). LLVM is under the Apache License v2.0 with LLVM Exceptions; each package includes its `LICENSE.TXT`.
