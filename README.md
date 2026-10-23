# Filip Filmar

I am a software engineer at Google.
In my own time I build programmable hardware, and the build systems around it.
Most of that work is open source, and I write about it on
[hdlfactory.com](https://www.hdlfactory.com).

This page lists my public work from October 2025 to October 2026.

## Hardware and RISC-V

- **[TxHDL](https://github.com/filmil/hdl-txhdl)** is a hardware description
  language written as a Rust library, built with Dragiša
  ([dj3maj](https://github.com/dj3maj)).
  A design is ordinary async Rust, and the library lowers it to Verilog and
  VHDL.
  - **Vreteno** is the RV32IMAC RISC-V core written in it.
    It runs at 100 MHz on an Artix-7 board with 1 GiB of DDR3.
    Mainline Linux boots on it to a BusyBox shell, with virtual memory on,
    and Doom runs on it with HDMI output.
  - **Razboj** is a minimal GPU in about a hundred lines of TxHDL.
    It reads a display list and draws rectangles and triangles into a
    framebuffer.
  - **[vreteno-conformance](https://github.com/filmil/vreteno-conformance)**
    runs RISC-V International's architectural tests (ACT4) on the newest
    core every day.
    The Sail reference model checks each result, and each daily report is
    published as a PDF.
- **[Cocoapuffs](https://github.com/filmil/cocoapuffs-fpga)** is a RISC-V
  system on chip on the same Artix-7 board, built around the 64-bit NOEL-V
  core.
  Fuchsia's Zircon kernel boots on it end to end, from OpenSBI to userspace.
- **[wdbcvt](https://github.com/filmil/wdbcvt)** reads Vivado's waveform
  database (`.wdb`), a format AMD does not document.
  It converts the files to FST and SQLite, and handles over 95% of the
  format's features, as measured on behavioral VHDL, Verilog and
  SystemVerilog.

## Bazel for programmable hardware

I build FPGA designs with [Bazel](https://bazel.build), from the toolchain
up, so that a fresh machine builds a bitstream with one command.

- **[rules_vivado](https://github.com/filmil/bazel_rules_vivado)** runs
  AMD Vivado synthesis, place and route, and programming as Bazel actions.
  [toolchains_vivado](https://github.com/filmil/bazel_toolchains_vivado)
  installs Vivado itself as an ephemeral toolchain.
- **[rules_openxc7](https://github.com/filmil/bazel_rules_openxc7)** does
  the same for 7-series parts with the open toolchain: Yosys, nextpnr-xilinx
  and Project X-Ray.
  It uses the rule names of rules_vivado, so a project switches between the
  two by changing one `load` line.
  I [measured the build times](https://www.hdlfactory.com/post/2026/10/08/openxc7-vs-vivado-build-times/)
  of the two side by side.
- **Simulation and libraries:**
  [rules_nvc](https://github.com/filmil/bazel_rules_nvc),
  [toolchains_nvc](https://github.com/filmil/bazel_toolchains_nvc),
  [toolchains_verilator](https://github.com/filmil/bazel_toolchains_verilator),
  [rules_vunit](https://github.com/filmil/bazel_rules_vunit),
  [rules_osvvm](https://github.com/filmil/bazel_rules_osvvm),
  [grlib](https://github.com/filmil/bazel_grlib) (Gaisler's GRLIB and
  NOEL-V), and [rules_ghdl](https://github.com/hw-bzl/rules_ghdl), where I
  merged 27 changes this year.
- **Firmware:** [rules_opensbi](https://github.com/filmil/bazel_rules_opensbi),
  [rules_dtc](https://github.com/filmil/bazel_rules_dtc) and a bare-metal
  [RISC-V GCC toolchain](https://github.com/filmil/bazel_rules_gcc_riscv64_baremetal).

## Hermetic builds

A hermetic build uses only tools it fetched itself, pinned by checksum, and
nothing installed on the host.

- **[bazel_local_nix](https://github.com/filmil/bazel_local_nix)** brings
  Nix packages into a Bazel build with no Nix installed on the host.
  It runs an ephemeral Nix, unprivileged, in a bubblewrap sandbox.
- **[bazel_rootfs](https://github.com/filmil/bazel_rootfs)** and
  **[xrootfs](https://github.com/filmil/xrootfs)** give a build a pinned
  Debian root file system, taken from an OCI image.
- **[bazel-bats](https://github.com/filmil/bazel-bats)**,
  **[bazel-ebook](https://github.com/filmil/bazel-ebook)**,
  **[rules_shar](https://github.com/filmil/rules_shar)**,
  **[rules_latex_host](https://github.com/filmil/bazel_rules_latex_host)**,
  **[bazel_graphviz](https://github.com/filmil/bazel_graphviz)** and
  **[gotopt2](https://github.com/filmil/gotopt2)** are smaller rule sets and
  tools.
  I published 43 releases of these modules to the Bazel Central Registry
  this year.

## Contributions to other projects

- **[google/rust_icu](https://github.com/google/rust_icu)**, the Rust
  bindings for ICU, which I maintain: 52 merged changes this year.
  I moved the build to Bazel, added ICU 74 to 77, and automated the
  releases.
  I also fixed two memory safety bugs in message formatting: a read past
  the end of the arguments, and strings passed without a terminating null.
- **Smaller fixes:** a universal macOS binary for the
  [rain](https://github.com/cenkalti/rain) BitTorrent client, support for
  the Alinx AX7A200B board in [fusesoc/blinky](https://github.com/fusesoc/blinky),
  and a documentation fix in
  [uber/hermetic_cc_toolchain](https://github.com/uber/hermetic_cc_toolchain).

## Other projects

- **[synod](https://github.com/filmil/synod)** is a Paxos agent in Go.
  It keeps a key-value store in agreement across peers that join at any
  time.
- **[mkill](https://github.com/filmil/mkill)** watches memory use.
  When memory runs low, it stops the program whose use grew fastest, before
  the kernel's out-of-memory killer has to act.
- **[chrome-auto-reload](https://github.com/filmil/chrome-auto-reload)** is a
  Chrome extension that reloads a tab on a timer.
  It has no access to page content and makes no network requests.

## Writing

I published 38 articles on [hdlfactory.com](https://www.hdlfactory.com)
since October 2025.
A selection:

- [TxHDL: a hardware description language is just a Rust async runtime](https://www.hdlfactory.com/post/2026/09/12/txhdl-a-hardware-description-language-that-is-a-rust-library/)
- [Linux boots to a shell on Vreteno](https://www.hdlfactory.com/post/2026/10/05/linux-boots-on-vreteno/)
- [Doom on a RISC-V core written in TxHDL](https://www.hdlfactory.com/post/2026/10/08/doom-on-a-txhdl-risc-v-core/)
- [Booting Fuchsia's Zircon on a homebrew RISC-V SoC, on an Artix-7 FPGA](https://www.hdlfactory.com/post/2026/07/19/cocoapuffs-booting-fuchsias-zircon-kernel-on-a-risc-v-core-in-artix-7-fpga/)
- [Bazel all the way down: how I build programmable hardware](https://www.hdlfactory.com/post/2026/08/17/bazel-all-the-way-down-how-i-build-programmable-hardware/)
- [My homelab is fully ephemeral](https://www.hdlfactory.com/post/2026/08/30/fully-ephemeral-homelab/)
- [The FPGA hardware interview: a topic guide with questions and exercises](https://www.hdlfactory.com/post/2026/07/23/fpga-hardware-interview-guide/)
- Fuchsia Internals, a report series for hardware engineers:
  [Vol. I, graphics and display](https://www.hdlfactory.com/post/2026/08/25/fuchsia-internals-vol-1-graphics-and-display/),
  and [Vol. II, the build system and toolchains](https://www.hdlfactory.com/post/2026/09/25/fuchsia-internals-vol-2-build-system-and-toolchains/).
- [Thanklessly Maintaining Open Source Software](https://www.hdlfactory.com/post/2026/10/04/thanklessly-maintaining-open-source-software/),
  a short humor book about a year in the life of a maintainer.

## Skills

- **Languages:** Rust, Go, C++, Python, Starlark, shell, VHDL,
  Verilog and SystemVerilog.
- **Hardware:** AMD 7-series FPGAs, Vivado, Yosys and nextpnr, Verilator,
  NVC and GHDL, RISC-V cores and boot chains (OpenSBI, Linux, Zircon).
- **Build and operations:** Bazel and bzlmod, Nix, Kubernetes, GitHub
  Actions and Forgejo Actions.

## Also

I am a licensed real estate agent in California.
My real estate site is [re.filmar.us](https://re.filmar.us).

## Contact

- **Website:** [hdlfactory.com](https://www.hdlfactory.com)
- **GitHub:** [filmil](https://github.com/filmil)
- **LinkedIn:** [in/filmil](https://www.linkedin.com/in/filmil/)
