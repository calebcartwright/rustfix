# rustfix

> **HIGHLY EXPERIMENTAL – MIGHT EAT YOUR CODE**

The goal of this tool is to read and apply the suggestions made by rustc (and third-party lints, like those offered by [Clippy][clippy]).

[clippy]: https://github.com/Manishearth/rust-clippy

## Current state

This tool can

- read a file of diagnostics (one JSON object per line)
- interactively step through the suggestions and ask the user what to do
- apply suggestions (currently whole lines only)

![rustfix demo](http://i.imgur.com/E9YkK76.png)

## Installation

Assuming you have a recent Rust nightly and Cargo installed:

```sh
$ cargo install --git https://github.com/killercup/rustfix.git
```

Make sure the binaries installed by Cargo are in your `$PATH`.

## Usage

In your project directory, just execute `rustfix`!

You probably want to use `rustfix --clippy` to get all the suggestions from [Clippy][clippy] as well. Make sure you have `cargo clippy` installed (`cargo install clippy`).

Please note that running `rustfix` multiple times in a project where no file was changed in the meantime will currently not generate any suggesions (as Cargo/Rust will skip the unchanged code and not compile it again).

### CLI Options

```plain
rustfix 0.1.0
Automatically apply suggestions made by rustc

USAGE:
    rustfix [FLAGS] [OPTIONS]

FLAGS:
        --clippy     Use `cargo clippy` for suggestions
    -h, --help       Prints help information
    -V, --version    Prints version information

OPTIONS:
        --from-file <FILE>    Read suggestions from file (each line is a JSON object)
```

## Get the example running

My current example output for diagnostics is based on [libui-rs](https://github.com/pcwalton/libui-rs). You can find the example JSON in `tests/fixtures/libui-rs/clippy.json`.

Run `rustfix`:

```sh
$ cd tests/fixtures/libui-rs/
$ cargo run -- --from-file clippy.json
```

### Generate the example diagnostics JSON yourself

```sh
$ git clone https://github.com/pcwalton/libui-rs.git
# HEAD is at 13299d28f69f8009be8e08e453a9b0024f153a60
$ cd libui-rs/ui/
$ cargo clippy -- -Z unstable-options --error-format json &2> clippy.json
# Manually remove the first line ("Compiling....")
```

## Gotchas

- rustc JSON output is unstable
- Not all suggestions can be applied trivially (e.g. clippy's "You should use `Default` instead of that `fn new()` you just wrote"-lint will replace your `new`-method with an `impl` block -- which is obvisouly invalid syntax.)
- This tool _will_ eat your laundry

## License

Licensed under either of

- Apache License, Version 2.0, ([LICENSE-APACHE](LICENSE-APACHE) or <http://www.apache.org/licenses/LICENSE-2.0>)
- MIT license ([LICENSE-MIT](LICENSE-MIT) or <http://opensource.org/licenses/MIT>)

at your option.

### Contribution

Unless you explicitly state otherwise, any contribution intentionally
submitted for inclusion in the work by you, as defined in the Apache-2.0
license, shall be dual licensed as above, without any additional terms or
conditions.