# RAMP from scratch

This is a rewrite of the RAMP (Rapid Assistance in Modelling the Pandemic)
model, based on the
[EcoTwins-withCommuting branch](https://github.com/Urban-Analytics/RAMP-UA/tree/Ecotwins-withCommuting),
in Rust.

Only the initialisation phase, which builds up a cache per study area, is ported
right now.

## Running the code

You will first need to install the latest version of Rust (1.57):
<https://www.rust-lang.org/tools/install>

You can then build and run the project:

```shell
git clone https://github.com/dabreegster/rampfs/
cd rampfs
# This will take a few minutes the first time you do it, to build external dependencies
cargo build --release
# The executable on Linux and Mac is in ./target/release/ramp. On Windows, you
# probably have to add .exe
./target/release/ramp west-yorkshire-small
```

This will download some large files the first time. If all succeeds, you should
have `processed_data/WestYorkshireSmall.bin` as output, as well as lots of
intermediate files in `raw_data/`.

(Note it'll fail the first time through and prompt you to manually run a Python
script to get some QUANT data in a different format.)

### Troubleshooting

The code depends on [proj](https://proj.org) to transform coordinates. You may
need to install additional dependencies to build it, like `cmake`. Please
[open an issue](https://github.com/dabreegster/rampfs/issues) if you have any
trouble!

### Some tips for working with Rust

There are two equivalent ways to rebuild and then run the code. First:

```shell
cargo run --release -- devon
```

The `--` separates arguments to `cargo`, the Rust build tool, and arguments to
the program itself. The second way:

```shell
cargo build --release
./target/debug/ramp devon
```

You can build the code in two ways -- **debug** and **release**. There's a
simple tradeoff -- debug mode is fast to build, but slow to run. Release mode is
slow to build, but fast to run. For the RAMP codebase, since the input data is
so large and the codebase so small, I'd recommend always using `--release`. If
you want to use debug mode, just omit the flag.
