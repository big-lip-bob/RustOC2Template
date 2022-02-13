# RustOC2Template
A template to have a ready and working environment to get Rust up and running on OpenComputers 2 RISC-V architecture

Currently it only provides a setup for RISC-V Linux MUSL and requires a RISC-V C toolchain
I picked said toolchain from [Bootlin](https://toolchains.bootlin.com/releases_riscv64-lp64d.html) (must be MUSL) (went with bleeding edge)
Be sure to run `relocate-sdk.sh`, and use the `buildroot` variant of the linker. (and also specify the correct path in `config.toml` if you plan to have the toolchain in a separate place than i do (`./../toolchain/riscv64-lp64d-musl-bleeding-edge-2021.11-1`))

Aside from that, the `cargo.toml` flags are optimized for binary size mostly, but they require the nightly toolchain and `std` is enabled and built from `nightly-x86_64-unknown-linux-gnu` (had to go bump the used `libc` version to `217` as it introduces a patch for missing `SYS` constants).
