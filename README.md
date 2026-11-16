# Miatoll Kernel Builder

GitHub Actions workflow to build custom kernels for **Xiaomi Miatoll (SM6250)**, based on `yukihanazuki/miatoll_neko-kernel` branch `16`.

## How it works

This repo **does not contain the kernel source** itself. The workflow clones your fork
`yukihanazuki/miatoll_neko-kernel`@`16` at build time, so the original fork remains
untouched.

## Triggering a build

1. Go to the **Actions** tab in your new repo (`miatoll-kernel-builder`).
2. Click **Build Miatoll Kernel**.
3. Fill the inputs:
   - **Kernel name** – custom name that appears in the zip filename and kernel suffix.
   - **Perf preset** – choose:
     - `performance` → tuned for maximum CPU throughput
     - `balanced` → middle-ground (SCHEDUTIL governor)
     - `battery` → powersave governor
   - **Release** – tick to also publish a GitHub Release with the zip as an asset.
4. Click **Run workflow**.

## Artifacts & Releases

- After the run finishes, download the compiled zip from the **Artifacts** section
  (or from the Release if you ticked the option).
- The zip filename format: `<kernel_name>-miatoll-YYYYMMDD-HHMM.zip` (e.g. `MiatollKernel-miatoll-20261116-2230.zip`).
- The kernel suffix in `Image.gz` filename will be `-<kernel_name>` (set via `LOCALVERSION`).

## Under the hood

- **Toolchain** (mirrors the proven build used by the device's own build scripts):
  - AOSP clang `r530567` (downloaded from `userariii/AOSP-clang` releases)
  - LineageOS `gcc 4.9` cross compilers (aarch64 and arm)
- **Defconfig**: `vendor/xiaomi/miatoll_defconfig` from your fork
- **DTB**: `arch/arm64/boot/dts/qcom/cust-atoll-ab.dtb`
- **Performance tweaks**: applied via `scripts/config` (CPU freq governor + DEBUG_INFO/KERNEL toggles)
- **Packaging**: AnyKernel3 from `osm0sis/AnyKernel3` (master), zips Image.gz + dtbo.img + relevant DTBs
- The build runs on `ubuntu-latest` runner

## Repository safety

- The workflow **only reads** from your fork (`yukihanazuki/miatoll_neko-kernel`). No pushes are made to it.
- All build artifacts go into this builder repo only.

## License

MIT (or choose whatever you prefer). Feel free to modify the workflow for your own needs.