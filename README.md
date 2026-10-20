# i1n1

Build-only superbuild that resolves and builds the five module repositories in dependency order.

Experimental, build-only source release: sources, headers and CMake files for the libraries and tools
of this module. No tests, firmware, Apple software or guest images are included, and no complete guest
boot is claimed.

## Repositories

- `i1n1-core` — Public C API headers, shared CMake policy, build identity and the ONXT bridge/driver-RPC codec library (https://github.com/i1n1/i1n1-core.git)
- `i1n1-engine` — AArch64 interpreter/MMU/PAC engine libraries and native broker (standalone CMake project, no i1n1 dependencies) (https://github.com/i1n1/i1n1-engine.git)
- `i1n1-platform` — x64 host memory/EPT model, platform device models, guest boot contract, cached DBT and boot glue (https://github.com/i1n1/i1n1-platform.git)
- `i1n1-driverzone` — Driver zone: simulated NVMe/xHCI devices, VMX root-mode skeleton and driver RPC shim (https://github.com/i1n1/i1n1-driverzone.git)
- `i1n1-host-efi` — x64 UEFI host layer: boot runner library and the freestanding i1n1.efi application build (https://github.com/i1n1/i1n1-host-efi.git)

## Build

```sh
# Check out the five repositories side by side, then:
cmake -S i1n1 -B build -G Ninja -DCMAKE_BUILD_TYPE=RelWithDebInfo \
  -DI1N1_CORE_SOURCE_DIR=$PWD/i1n1-core -DI1N1_ENGINE_SOURCE_DIR=$PWD/i1n1-engine \
  -DI1N1_PLATFORM_SOURCE_DIR=$PWD/i1n1-platform -DI1N1_DRIVERZONE_SOURCE_DIR=$PWD/i1n1-driverzone \
  -DI1N1_HOST_EFI_SOURCE_DIR=$PWD/i1n1-host-efi
cmake --build build
```

Without the `*_SOURCE_DIR` overrides the repositories are fetched at the commits pinned in
`cmake/i1n1_deps.cmake`.

## Supported hosts and limitations

- Verified build environment: macOS on arm64, Apple clang 21, CMake 4.4, Ninja, C11, `BUILD_TESTING=OFF`.
  Other hosts and compilers have not been verified for this release.
- Warnings are errors by default (`-DI1N1_WARNINGS_AS_ERRORS=OFF` relaxes this for other compilers).
- Unsupported instructions, registers or devices are reported as deterministic stops by the source, not
  emulated; this release is a build of that source, not evidence of a working guest.

## License

GPL-2.0-only for project-authored material; see `COPYING` and `LICENSE`.
