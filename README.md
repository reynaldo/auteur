![Auteur CI](https://github.com/MathieuDuponchelle/auteur/actions/workflows/CI.yml/badge.svg?branch=main)

# Auteur

The current scope of this project is that of a basic live node
compositor, with a service exposing a JSON API for creating,
connecting, inspecting, scheduling and removing GStreamer processing
nodes (sources, mixers and destinations as of writing).

It can be used for things like linear video feeds that allow hotswapping inputs/sources and compositing
them based on a schedule (cue time and duration). As well as fail-over to backup content when a source 
is unavailable.

An example client running commands one at a time is provided
for exploring that API.

[Read the documentation](https://microsoft.github.io/auteur)

## Environment

This project depends on:

* Rust (stable channel)

* GStreamer (master as of writing)

* gst-plugins-rs (master as of writing)

## Building

The most convenient testing platform for this PoC is a Linux machine.

A few GStreamer plugins are needed, make sure to install the dependencies
for all of those before building gstreamer:

``` shell
git grep "make_element(" | cut -d '"' -f 2 | sort -u
```

Checkout the the new <https://gitlab.freedesktop.org/gstreamer/gstreamer>
monorepo and build it with <https://gitlab.freedesktop.org/gstreamer/gst-plugins-rs>
enabled explicitly:

``` shell
git clone https://gitlab.freedesktop.org/gstreamer/gstreamer
cd gstreamer
meson -Drs=enabled build
ninja -C build
ninja -C build devenv
```

Follow the rest of these instructions by first changing directory to your Auteur checkout from within the GStreamer development environment you entered with the previous command.

``` shell
cargo build
```

## Running

Run the service:

``` shell
AUTEUR_LOG=debug cargo run --bin auteur
```

Explore and test the API with the client:

``` shell
cargo run --bin auteur-controller -- help
```

You can also find the API definition in `common/src/controller.rs`.

In addition, a simple wrapper script around the controller can
be found in `scripts/node_schedule.py`, alongside other scripts that leverage
it to demonstrate other features (eg `scripts/crossfade.py`)

## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.

## Trademarks

This project may contain trademarks or logos for projects, products, or services. Authorized use of Microsoft 
trademarks or logos is subject to and must follow 
[Microsoft's Trademark & Brand Guidelines](https://www.microsoft.com/en-us/legal/intellectualproperty/trademarks/usage/general).
Use of Microsoft trademarks or logos in modified versions of this project must not cause confusion or imply Microsoft sponsorship.
Any use of third-party trademarks or logos are subject to those third-party's policies.

## License

All code in this repository is licensed under the [MIT license](LICENSE).
