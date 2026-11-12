# Bevy PBR, with PBR textures sampled without `let`s

This is `bevy_pbr` 0.20.1 as published on crates.io, with one change to its
shaders, for use through Cargo's `[patch]`:

```toml
[patch.crates-io]
bevy_pbr = { git = "https://github.com/semio-ai/bevy_pbr-safari-fixed", rev = "<commit>" }
```

The change makes `StandardMaterial` render on Safari's WebGPU. Bevy's PBR
shaders bind each material texture and sampler to a `let` before sampling it.
Core WGSL does not allow that; it needs the optional `texture_and_sampler_let`
language extension, which Chrome implements and Safari does not. Safari
therefore rejects the main mesh pipeline and draws nothing.

Here, when `BINDLESS` is off, each sample call names
`pbr_bindings::<x>_texture` and `pbr_bindings::<x>_sampler` directly. The
bindless branch is unchanged: it needs binding arrays, which WebGPU does not
expose, so it never reaches a browser. Everything else is the published crate,
under its original MIT or Apache-2.0 license.

Drop the patch once a Bevy release samples these textures without `let`s.

---

# Bevy PBR

[![License](https://img.shields.io/badge/license-MIT%2FApache-blue.svg)](https://github.com/bevyengine/bevy#license)
[![Crates.io](https://img.shields.io/crates/v/bevy_pbr.svg)](https://crates.io/crates/bevy_pbr)
[![Downloads](https://img.shields.io/crates/d/bevy_pbr.svg)](https://crates.io/crates/bevy_pbr)
[![Docs](https://docs.rs/bevy_pbr/badge.svg)](https://docs.rs/bevy_pbr/latest/bevy_pbr/)
[![Discord](https://img.shields.io/discord/691052431525675048.svg?label=&logo=discord&logoColor=ffffff&color=7389D8&labelColor=6A7EC2)](https://discord.gg/bevy)
