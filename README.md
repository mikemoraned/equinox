# Useful links found

https://pixelspark.nl/2020/cross-compiling-rust-programs-for-a-raspberry-pi-from-macos
https://dev.to/jeikabu/raspberry-pi-zero-raspbian-rust-primer-3aj6

## commands

    brew install FiloSottile/musl-cross/musl-cross --without-x86_64 --with-arm-hf --with-arm --with-aarch64

... but hit this problem: https://github.com/FiloSottile/homebrew-musl-cross/issues/23 (may need to wait until mid-2021 for a gcc that supports Apple Silicon)

trying following, which runs brew under intel emulation (no idea if this makes any difference)

    ibrew install FiloSottile/musl-cross/musl-cross --without-x86_64 --with-arm-hf --with-arm --with-aarch64

... this seemed to compile

    rustup target add arm-unknown-linux-musleabihf

    CROSS_COMPILE=arm-linux-musleabihf- cargo build --release --target arm-unknown-linux-musleabihf

    scp target/arm-unknown-linux-musleabihf/release/hello_from_pi pi@192.168.2.39:~/

on the pi zero:

    $ ./hello_from_pi
    Illegal instruction

so, doesn't work as-is

try stripping?

    arm-linux-musleabihf-strip target/arm-unknown-linux-musleabihf/release/hello_from_pi
    scp target/arm-unknown-linux-musleabihf/release/hello_from_pi pi@192.168.2.39:~/

(same)

trying some other stuff

    rustup target add arm-unknown-linux-musleabi
    CROSS_COMPILE=arm-linux-musleabi- cargo build --release --target arm-unknown-linux-musleabi
    scp target/arm-unknown-linux-musleabi/release/hello_from_pi pi@192.168.2.39:~/

on PI, it works!

    $ ./hello_from_pi
    Hello from PI!
