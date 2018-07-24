# Fractal Forest

Better Portable Graphics module for [After Dark]. Fractal Forest adds support for Fabrice Bellard's [well-regarded](https://news.ycombinator.com/item?id=17587684) and open source [BPG Image format](https://bellard.org/bpg/).

## Requirements

- [After Dark] `5.0.0` or later.

## Setup

None required. Simply fetch the included JS files into an HTML page and BPG images on the page will be discovered and rendered automatically. If you're considering shipping native [wasm](https://webassembly.org) to the browser head to the [Development](#development) section and see the [Testing native WebAssembly](https://kripken.github.io/emscripten-site/docs/compiling/WebAssembly.html#testing-native-webassembly-in-browsers) and [Web server setup](https://kripken.github.io/emscripten-site/docs/compiling/WebAssembly.html#web-server-setup) section in the Emscripten docs. Shipping native wasm to the browser affords streaming browser compilation but can increase debugging complexity.

## Installation

1. Copy the contents of this repository into a directory called `themes/fractal-forest` under the root of your After Dark site.
2. Add `fractal-forest` as a [theme component](https://gohugo.io/themes/theme-components/) to your After Dark site `config.toml`, e.g.

    ```toml
    theme = [
      "fractal-forest",
      "after-dark"
    ]
    ```

3. Add and specify settings for the module in your After Dark site config, e.g.

    ```toml
    [params.modules.fractal_forest]
      enabled = true # Optional, set false to disable module
      decoders = [
        "bpgdec8", # 8-bit only javascript decoder without animation
        "bpgdec", # > 8-bit javascript decoder without animation
        "bpgdec8a" # 8-bit javascript decoder with animation
      ]
      crossorigin = "anonymous" # Optional, sets CORS attribute
    ```

4. Build and deploy your After Dark site.

For additional information please see [BPG Image format](https://bellard.org/bpg/). To request a feature or report a bug please do so in the [After Dark] repository.

## Development

For development, install Docker on your machine:

- [Get started with Docker for Mac](https://docs.docker.com/docker-for-mac/)
- [Get started with Docker for Windows](https://docs.docker.com/docker-for-windows/)

Then build the codecs with [`docker build`](https://docs.docker.com/engine/reference/commandline/build/).

To adjust the version of bpg used simply modify `LIBBPG_VERSION` in the `Dockerfile` for desired version and rebuild. If you're on a multicore system adjust `CPU_CORES` to decrease compilation time.

Docker build produces an intermediate container image with libbpg source and result of compilation. It also copyies the codecs into a busybox image.

To access the full `libbpg` source run:

```sh
$ docker run -it 30c982469f98
```

Where `30c982469f98` is the image id of the intermediate step.

To access just the codecs run:

```sh
$ docker run -it 712e9ce47e86
```

Where `712e9ce47e86` is the image id of the final build step.

To update the javascript decoders in `static/js/bpg` run:

```
$ docker run --rm --entrypoint tar 712e9ce47e86 cC /var/www/ . | tar xvC static/js
```

Where `712e9ce47e86` is the image id of the final build step.

Reference the [`libbpg` mirror](https://codeberg.org/vhs/vhs/libbpg/) for additional compilation settings, `README` and `Makefile`. See the [Docker Documentation](https://docs.docker.com) for help with Docker.

## License

Copyright (C) 2018 VHS <0xc000007b@tutanota.com>

This work is free. You can redistribute it and/or modify it under the
terms of the Do What The Fuck You Want To Public License, Version 2,
as published by Sam Hocevar. See the COPYING file for more details.

[After Dark]: https://codeberg.org/vhs/after-dark/
