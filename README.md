**This is a fork from [r-hub/r-minimal](https://github.com/r-hub/r-minimal) with a few changes specific to rexyai usage**

**Credits to Gabor Csardi**

# Minimal Docker images for R

## What is R?

R is a free software environment for statistical computing and graphics.
It compiles and runs on a wide variety of UNIX platforms, Windows and
MacOS. See more at https://www.r-project.org/

## Goals and features

The main goal of these images is to keep them minimal, so they can be used
as part of a bigger (web) application, or as a base image. Currently the
`r-minimal` image is less than 20MB compressed, and 32MB uncompressed.

All images use Alpine Linux.

The images include the `installr` tools that can install R packages from
CRAN or GitHub.

## Limitations

To keep the images minimal, they do not include a number of parts and
features that most users would prefer to have for interactive R
development:

* Recommended R packages are not installed.
* Documentation is not included.
* No readline library support.
* No X11 support.
* No Java support.
* ~~No OpenMP support.~~
* No JPEG, PNG or TIFF support.
* No Cairo support.
* No Tcl/Tk support.
* No translations, only English.
* The image does not have C, C++ or Fortran compilers.

Changes in this repository:

* R shared library enabled
* R is compiled with OpenMP support


## Usage

Get the image from
[Docker Hub](https://hub.docker.com/repository/docker/rexyai/r-minimal):

```
docker pull docker.io/rexyai/r-minimal:latest
```

## Dockerfile examples

One of our main goals is to be able to use `rexyai/r-minimal` as a base
image, and easily add R packages from CRAN or GitHub to it, to create a
new image. Run `installr` from a `Dockerfile` to add R packages to
the `r-minimal` image:

Package with compiled code:

```
FROM rexyai/r-minimal
RUN installr -d glue
```

After the package(s) have been installed, `installr` removed the compilers,
as these are typically not needed on the final image. If you want to keep
them use `installr -c` instead of `installr -d`.

Package with system requirements:

```
FROM rexyai/r-minimal
RUN installr -d -t linux-headers pingr
CMD [ "R", "-q", "-e", "pingr::is_online() || stop('offline')" ]
```

Similarly to compilers, system packages are removed after the R packages
have been installed. If you want to keep (some of) them, use `installr -a`
instead of `installr -t`. (You can also mix the two.)

## Examples:

Hints on installing some R packages:

package                 | installr command                                          | ~ image size
----------------------- | --------------------------------------------------------- | -------------
data.table              | `installr -d -t zlib-dev data.table`                      |  35.8 MB
rsparse (with openblas) | see `examples/rsparse/Dockerfile`                         |  94.0 MB

See `examples` directory for more details.

## License

See https://www.r-project.org/Licenses/ for the R licenses

These Dockerfiles are licensed under the MIT License.

(c) [R Consortium](https://github.com/rconsortium)
