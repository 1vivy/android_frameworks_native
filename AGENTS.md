# PROJECT KNOWLEDGE BASE
**Project:** android_frameworks_native
**Seeded-at-ref:** crdroid/16.0
**Seeded-at-oid:** da1214c0a2625e98782acec189033c2974614fe4
**Seeded-at-date:** 2026-08-14
**Generated-policy-sha256:** 831fceec497e89cad3d18f57f71d7d9fbc2bf2498062efbc079821f326bd17f0

## UPSTREAM DISTILLATION

### Scope and ownership

This project owns Android's native framework foundations: Binder-facing native libraries, graphics buffers and windows, input and sensor services, SurfaceFlinger, OpenGL/EGL, and Vulkan. Keep policy and mechanism with the narrowest owner. Device-specific panel control belongs behind its hardware owner; SurfaceFlinger owns composition, scheduling, display modes, transactions, and expected-present behavior rather than raw panel feature IDs.

All path guidance below is `CURRENT_PATH` truth at the seeded OID, which is also this carrier's delivery parent. There are no seed-only path or command claims in this guide.

### Module and build graph map

- `Android.bp` is the root Soong entry and namespace surface; subsystem build graphs remain in their local `Android.bp` files.
- `libs/` contains reusable native framework libraries, including Binder, GUI, UI, input, native window, and sensor foundations. Public and VNDK-facing headers live under `include/`, `headers/`, and subsystem include directories.
- `services/surfaceflinger/` owns composition, scheduler, display-device state, layer transactions, tracing, and display policy. Its flags and sysprops are defined locally under `services/surfaceflinger/`.
- `services/inputflinger/` and `services/sensorservice/` own native input and sensor service behavior. Do not place those responsibilities in SurfaceFlinger.
- `services/vibratorservice/`, `services/powermanager/`, and the other focused service directories are framework clients or coordinators of their corresponding HALs, not replacements for those HALs.
- `opengl/` and `vulkan/` contain loader/runtime implementation and API-facing integration. Their root `Android.bp`, `OWNERS`, and `TEST_MAPPING` files define local build and test boundaries.
- `aidl/` contains native framework interface declarations. API changes must preserve interface/version compatibility and update the owning API artifacts where present.

### Interfaces, policy, and extension boundaries

This repository does not carry product sepolicy. New native Binder services require a complete cross-repository integration join: service implementation and registration here, interface ownership in the correct AIDL surface, product packaging, init/VINTF declarations where applicable, and policy in the owning system or device policy tree.

Prefer existing AOSP or Lineage typed contracts over private transactions. crDroid-specific behavior in this seed includes focused SurfaceFlinger and native-service changes; preserve upstream separation between scheduler policy, composition state, hardware-composer contracts, and device HAL state. A display extension must not leak panel IDs, packed values, sysfs paths, or firmware revisions through framework APIs.

### Verification and conventions

`TEST_MAPPING` is the repository-wide presubmit map; subsystem maps such as `opengl/TEST_MAPPING` and `vulkan/TEST_MAPPING` narrow ownership. Match nearby C++ or Rust formatting and existing error-handling conventions. Changes to a service are tested at its local unit/fuzzer boundary, then at the relevant framework/HAL integration surface; compilation alone does not prove service registration, scheduling behavior, or physical display state.

Recent downstream subjects use scoped forms such as `surfaceflinger: ...`, `build: ...`, and `native: ...`; preserve the nearest subsystem's imperative style. Avoid broad drive-by formatting in this large shared framework project.

## OUR DELTAS

None at seed. Later entries must name topic commit OIDs and must not rewrite upstream truth.
