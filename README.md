Set up Cygwin
=============

[![Test](https://github.com/egor-tensin/setup-cygwin/workflows/Test/badge.svg)](https://github.com/egor-tensin/setup-cygwin/actions?query=workflow%3ATest)

This is a GitHub action that sets up Cygwin in your workflow run.

1. Installs Cygwin.
2. Installs any additional packages specified.

Use it in your workflow like this:

    - name: Set up Cygwin
      uses: egor-tensin/setup-cygwin@v2
      with:
        platform: x64
        packages: cmake python3

* `x64` is the default value for the `platform` parameter and can be omitted.
Use `x86` if you want to install the 32-bit Cygwin.
* Specify any additional packages to be installed as a space-separated list in
the `packages` parameter.
* Set the installation directory using the `install-dir` parameter
(C:\tools\cygwin is the default).
* The `CYGWIN` environment variable is set to an empty string by default.
Provide a custom value using the `env` parameter.
* Some packages install symlinks in /usr/bin along with real executables.
An example is package python3, which (as of January 2021) installs symlink
"python3", pointing to the real executable "python3.8.exe".
Calling Cygwin symlinks from Windows' command prompt is unsupported, but
there's an option to convert them to hardlinks instead (the
`symlinks-to-hardlinks` parameter).

API
---

| Input                 | Value   | Default | Description
| --------------------- | ------- | ------- | -----------
| platform              | x64     | Yes     | Install the 64-bit version.
|                       | *Other* | No      | Install the 32-bit version.
| install-dir           | *Empty* | Yes     | Install to C:\tools\cygwin.
|                       | *Other* | No      | Install to the specified directory.
| packages              | *Empty* | Yes     | Don't install any additional packages.
|                       | *Other* | No      | Space-separated package names.
| env                   | *Empty* | Yes     | Set `%CYGWIN%` to an empty string.
|                       | *Other* | No      | Set `%CYGWIN%` to the specified value.
| symlinks-to-hardlinks | *Other* | Yes     | Don't convert any symlinks.
|                       | 1       | No      | Convert symlinks in /usr/bin to hardlinks.

The paths to the Cygwin binaries are added to the PATH variable.

License
-------

Distributed under the MIT License.
See [LICENSE.txt] for details.

[LICENSE.txt]: LICENSE.txt
