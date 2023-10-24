## Enabling `gdb`

* In `Embed.toml`, set `default.gdb.enabled = true`.

* Now, `cargo embed` (*without* `--release`) will put up a
  message about a "GDB stub".
  
* The idea here is that there is a proxy running on the host
  talking to the debug stuff on the nRF53820 debugging chip
  on the MB2. Once you connect a GDB to the proxy, you can
  debug as if you are running the MB2 code on an OS and
  debugging with GDB there.

* Start `rust-gdb` on the target executable. Yes, this
  will work. The "rust-" part enables some GDB support for
  printing Rust data.

* Once in GDB, say `target remote :1337`. This will connect
  GDB to the proxy.

## Rust printf Debugging

* Rust doesn't have `printf()`. Has `eprintln!()` tho.

* Rust doesn't even have `eprintln!()` when `no_std`.

* Two problems:

  * How to get `println!()` capabilities into embedded Rust

  * Where does the output go? How do I read it?
  
## Enabling `rtt`


* First, add printing support:

  * In `Embed.toml`:
  
    ```toml
    [default.rtt]
    enabled = true
    ```

  * In `Cargo.toml`:

    ```
    [dependencies]
    cortex-m-rt = "0.7"
    embedded-hal = "0.2"
    microbit-v2 = "0.13"
    rtt-target = "0.4"

    [dependencies.panic-rtt-target]
    version = "0.1"
    features = ["cortex-m"]

    [dependencies.cortex-m]
    version = "0.7"
    features = ["inline-asm", "critical-section-single-core"]
    ```

    Note that this differs a bit from the book. See this
    [bug
    report](https://ferrous-systems.com/blog/defmt-rtt-linker-error/)
    for the terrible explanation.

  * At program top:

    ```rust
    use rtt_target::{rtt_init_print, rprintln};
    use panic_rtt_target as _;
    ```

  * At start of program:
  
    ```rust
    rtt_init_print!();
    ```
    
  * When needed, you can then use `rprintln!()`.

## ITM, defmt

* `ITM` = "Instrumentation Trace Macrocell", an RTT
  alternative.

* Semihosting: used by QEMU / emulators etc to print.

* `defmt` = "DEFerred ForMaTting", a fancier embedded
  logging crate. Look up the docs on how to use: I never
  have used it. Can run atop ITM, defmt, semihosting.
  
## Serial through USB

* I had bought a fancy USB-serial card to hook up to the
  Dragon Tail. Worked great with little messing.
  
* Then realized that stuff written out the default serial
  port `UARTE0` just comes out the USB port via
  `/dev/ttyACM0` on Linux.

* Once set up, can use `minicom` on Linux to talk to serial,
  use it as a console.
