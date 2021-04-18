Set up Cygwin
=============

[![Test](https://github.com/egor-tensin/setup-cygwin/actions/workflows/test.yml/badge.svg)](https://github.com/egor-tensin/setup-cygwin/actions/workflows/test.yml)

This GitHub action sets up Cygwin in your workflow run.

1. Installs Cygwin.
2. Installs any additional packages specified.

Use it in your workflow like this:

    - name: Set up Cygwin
      uses: egor-tensin/setup-cygwin@v3
      with:
        platform: x64
        packages: cmake python3

    # Cygwin executables are added to PATH, so you can call them directly:
    - run: |
        ls.exe -Al
        pwd.exe

    # Alternatively, you can use Cygwin's bash as shell to write proper bash scripts:
    - run: |
        basic() {
            ls -Al
            pwd
        }
        basic
      shell: C:\tools\cygwin\bin\bash.exe --login --norc -eo pipefail -o igncr '{0}'

* `x64` is the default value for the `platform` parameter and can be omitted.
Use `x86` if you want to install the 32-bit Cygwin.
* Specify any additional packages to be installed as a space-separated list in
the `packages` parameter.
* Set the installation directory using the `install-dir` parameter
(C:\tools\cygwin is the default).
* The `CYGWIN` environment variable is set to an empty string by default.
Provide a custom value using the `env` parameter.

API
---

| Input       | Value   | Default | Description
| ----------- | ------- | ------- | -----------
| platform    | x64     | ✓       | Install the 64-bit version.
|             | *any*   |         | Install the 32-bit version.
| install-dir | *empty* | ✓       | Install to C:\tools\cygwin.
|             | *any*   |         | Install to the specified directory.
| packages    | *empty* | ✓       | Don't install any additional packages.
|             | *any*   |         | Space-separated package names.
| env         | *empty* | ✓       | Set `%CYGWIN%` to an empty string.
|             | *any*   |         | Set `%CYGWIN%` to the specified value.
| hardlinks   | *any*   | ✓       | Don't convert any symlinks.
|             | 1       |         | Convert symlinks in /usr/bin to hardlinks.

The paths to the Cygwin binaries are added to the PATH variable.

Notes
-----

1. Some packages install symlinks in /usr/bin along with real executables.
An example is package "python3", which (as of January 2021) installs symlink
`python3`, pointing to the real executable `python3.8.exe`.
Calling Cygwin symlinks from Windows' command prompt is unsupported, but might
be convenient (e.g. if you don't know the exact Python version), so there's an
option to convert them to hardlinks instead (the `hardlinks` parameter).

License
-------

Distributed under the MIT License.
See [LICENSE.txt] for details.

[LICENSE.txt]: LICENSE.txt
