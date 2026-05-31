# hello-rs-libretro

A minimal [libretro](https://www.libretro.com/) core written in Rust, built as a learning
exercise and developer reference. If you want to write a libretro core in Rust and don't
know where to start, this is for you — and for future me.

This core displays an animated color-cycling crosshatch pattern and plays a sweeping tone
whose waveform can be cycled between square, pulse, and sawtooth by pressing a button. It
requires no ROM or content file to run.

## What this demonstrates

- The complete set of mandatory libretro exports and what each one does
- How to implement the libretro glue layer directly in Rust without wrapper crates,
  using raw `extern "C"` functions against the libretro ABI
- How to push video frames and audio samples to RetroArch each frame
- How to read input via RetroArch's poll/state callback model
- How to declare a no-content core that runs without a ROM file
- How to set the pixel format correctly (and why getting it wrong produces stripes)
- How to deploy a custom core to RetroArch on Steam on Linux

## Why no wrapper crates?

Several Rust libretro wrapper crates exist (`libretro-rs`, `rust-libretro`,
`libretro-backend`) but all are effectively unmaintained as of 2025. Rather than depend
on an abandoned abstraction, this project implements the libretro glue layer directly.
The API surface is small enough that this is straightforward, and it means you own every
inch of the stack.

## Project structure

```
src/
  lib.rs       — global statics and module declarations
  types.rs     — #[repr(C)] structs and constants mirroring libretro.h
  libretro.rs  — all extern "C" exports required by the libretro ABI
  core.rs      — HelloCore struct and runtime state
```

## The mandatory exports

RetroArch will refuse to load a core that is missing any of these symbols. The full list,
in the order RetroArch calls them during startup:

| Function | Purpose |
|---|---|
| `retro_set_environment` | Receive the environment callback; declare core capabilities |
| `retro_set_video_refresh` | Receive the video frame callback |
| `retro_set_audio_sample` | Receive the single-sample audio callback |
| `retro_set_audio_sample_batch` | Receive the batched audio callback |
| `retro_set_input_poll` | Receive the input poll callback |
| `retro_set_input_state` | Receive the input state callback |
| `retro_init` | Initialize core state |
| `retro_get_system_info` | Provide core name, version, supported extensions |
| `retro_get_system_av_info` | Provide resolution, aspect ratio, framerate, sample rate |
| `retro_load_game` | Load content (or accept null for no-content cores) |
| `retro_run` | Run one frame of video, audio, and input |
| `retro_unload_game` | Unload content |
| `retro_deinit` | Free core state |
| `retro_api_version` | Return `1` (RETRO_API_VERSION) |
| `retro_get_region` | Return NTSC (`0`) or PAL (`1`) |
| `retro_serialize_size` | Return save state buffer size, or `0` to opt out |
| `retro_serialize` | Write save state, or return `false` to opt out |
| `retro_unserialize` | Restore save state, or return `false` to opt out |
| `retro_get_memory_data` | Return pointer to a memory region, or null |
| `retro_get_memory_size` | Return size of a memory region, or `0` |
| `retro_cheat_reset` | Clear all active cheats |
| `retro_cheat_set` | Apply or remove a cheat code |
| `retro_set_controller_port_device` | Handle controller type changes |
| `retro_reset` | Soft reset |
| `retro_load_game_special` | Load multi-file content, or return `false` |

If RetroArch fails to load your core, check its log file for `Failed to load symbol:`
messages — that is how you find out which exports are missing.

## Hard-won lessons

**Set the pixel format explicitly.** If you do not call
`RETRO_ENVIRONMENT_SET_PIXEL_FORMAT` with `RETRO_PIXEL_FORMAT_XRGB8888` (value `1`),
RetroArch assumes the legacy `XRGB1555` format and your framebuffer colors will be
completely wrong. The correct constant values are:

| Constant | Value |
|---|---|
| `RETRO_PIXEL_FORMAT_0RGB1555` | `0` (legacy default) |
| `RETRO_PIXEL_FORMAT_XRGB8888` | `1` |
| `RETRO_PIXEL_FORMAT_RGB565` | `2` |

**Set the pixel format in `retro_set_environment`, not `retro_init`.** In some RetroArch
versions `retro_set_environment` is called after `retro_init`, which means the environment
callback is not yet available when `retro_init` runs. Setting the pixel format in
`retro_set_environment` ensures it is always applied.

**Pitch is bytes per row, not pixels per row.** The pitch argument to the video refresh
callback is the number of bytes between the start of one row and the start of the next.
For a packed XRGB8888 buffer this is `width * 4`.

**`retro_load_game_special` and `retro_reset` are required.** They are not in some
versions of the libretro documentation but RetroArch will refuse to load your core without
them.

**Use `#[unsafe(no_mangle)]` not `#[no_mangle]`.** As of Rust 1.82, `#[no_mangle]`
requires an explicit `unsafe` acknowledgment. Use `#[unsafe(no_mangle)]` on all exports.

## Deploying to RetroArch on Steam (Linux)

Build the core:

```bash
cargo build --release
```

Copy the `.so` to RetroArch's cores directory:

```bash
cp target/release/libhello_rs_libretro.so \
   ~/.steam/steam/steamapps/common/RetroArch/cores/
```

Create an `.info` file in RetroArch's info directory with a filename matching the `.so`:

```
~/.steam/steam/steamapps/common/RetroArch/info/libhello_rs_libretro.info
```

With these contents:

```
library_name = "hello-rs-libretro"
display_name = "hello-rs-libretro"
supported_extensions = ""
supports_no_game = "true"
```

In RetroArch: **Load Core → select the core → Start Core**. No content required.

## Building

```bash
cargo build          # debug build
cargo build --release  # release build
```

The `.so` will be at `target/debug/libhello_rs_libretro.so` or
`target/release/libhello_rs_libretro.so`.

## Documentation

```bash
cargo doc --open
```

All public items are documented. The generated docs include full descriptions of every
libretro callback, struct field, and constant.

## License

Copyright (C) 2025 David Brinovec

This program is free software: you can redistribute it and/or modify it under the terms
of the GNU Affero General Public License as published by the Free Software Foundation,
either version 3 of the License, or (at your option) any later version.

See [LICENSE](LICENSE) for the full license text.
