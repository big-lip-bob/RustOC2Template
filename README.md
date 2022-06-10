# RustOC2Template
A template to have a ready and working environment to get Rust up and running on [OpenComputers 2](https://github.com/fnuecke/oc2) [RISC-V architecture](https://github.com/fnuecke/sedna)

Currently it only provides a setup for RISC-V Linux MUSL and requires a RISC-V C toolchain.

I picked said toolchain from [Bootlin](https://toolchains.bootlin.com/releases_riscv64-lp64d.html) (must be MUSL) (went with bleeding edge)
Be sure to run `relocate-sdk.sh`, and use the `buildroot` variant of the linker (already specified in `config.toml`).
(and also specify the correct path in `config.toml` if you plan to have the toolchain in a separate place than i do (`$CRATE_ROOT/../toolchain/riscv64-lp64d-musl`))

It should be possible to also use Zig's `cc`  as its cross-platform by design,
but i sadly haven't had time to look into Zig and its amazing funsies
See [cargo-zigbuild](https://github.com/messense/cargo-zigbuild) and the blogpost about [Zig's linker in Rust](https://actually.fyi/posts/zig-makes-rust-cross-compilation-just-work)

Aside from that, the `Cargo.toml` flags are optimized for binary size mostly, but they require the nightly toolchain and `std` is built from `nightly-x86_64-unknown-linux-gnu`.
The `release` profile has been tweaked to abort (unwinding seems unavailable (the linker is confused))
thus its the only profile you should use (and you better as it has symbol stripping and other flags helping reduce the binary size).

## Reducing Binary Size
Avoiding `std` (and especially any `fmt`) massively helps get everything down but that's often not possible or maintainable.
`#![no_std]` can help in extreme cases altough its so versatile (and supported within the Linux image) that getting around may be difficult.

You should instead favour Dynamic dispatching, works well to reduce the binary size even further
Dynamic shared libraries are another option (`libloading` and our beloved `dlopen`) but

I am still figuring out how to make shared libraries in Rust and i do plan making some wrappers for my OC2 libraries to work with that
aswel as extracting the Rust runtime (if there is one) into its own shared library.

Relying on very big crates that heavely rely on generics and code generation such as `serde` and `clap` might also hurt binary size hence dynamic dispatching suggestions.

Besides that, to import your executable in-game, you can add a File Import/Export Card to your computer and use `import.lua`,
be mindful that the transfer speeds are very slow (~1kB/s) (and a Rust rewrite is 25x faster)
(I'll publish my Import/Exporter soon)

Requires Nightly Rust
