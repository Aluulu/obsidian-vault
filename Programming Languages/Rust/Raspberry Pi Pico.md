

# Creating a new Rust program

## Installing required Crates

Use the following commands to install and add these crates to your project:

```Shell
# Add the HAL (Hardware Abstraction Layer),
cargo add rp-pico
```
```Shell
# Required for the `#[entry]` macro
cargo add cortex-m-rt
```
```Shell
# panic-halt_ creates a simple panic function, which just halts
cargo add panic-halt
```
```Shell
# Rust implementation of the boot2 bootloader for the Raspberry Pi RP2040 microcontroller
cargo add rp2040_boot2
```

## Installing additional crates

You may find it useful to add these additional crates

```Shell
# Provides a set of traits and abstractions that can be used to write hardware-independent code
cargo add embedded_hal
```
```Shell
# Collection of low-level Rust libraries that provide access to the Cortex-M processor and its hardware peripherals
cargo add cortex_m
```
```Shell
# Specific HAL for the Raspberry Pi RP2040 microcontroller in Rust
cargo add rp2040_hal
```

## Copy cargo config and memory.x

Copy the config file located [here](https://raw.githubusercontent.com/rp-rs/rp-hal-boards/main/.cargo/config) to `config` located in the hidden folder `.cargo`. For example this may be `~/Documents/Pico-Rust/.cargo/config`. This specifies the target and optimizing flags to the linker.

Copy the file named `memory.x` to your project's root directory. This file tells the linker the flash and RAM layout, so it won't clobber the bootloader or write to an out of bounds memory address.

# Templates

## Blank template

```Rust
//! # I love you LED blink
//!
//! This application blinks the Pico LED to the morse code of "I love you".
//!
//! Created by Callum Wellard
  
// no_std and no_main are required for bare-metal applications
// This ensures that the standard library and entry point are not used (aka main)
#![no_std]
#![no_main]

// Ensure we halt the program on panic (if we don't mention this crate it won't
// be linked)
use panic_halt as _;

// Alias for our HAL crate
use rp2040_hal as hal;

// A shorter alias for the Peripheral Access Crate, which provides low-level
// register access
use hal::pac;

// Import traits
use embedded_hal::digital::v2::OutputPin; // Provides a common interface for controlling digital output pins
use rp2040_hal::clocks::Clock; // Provides a common interface for working with clocks

/// The linker will place this boot block at the start of our program image. We
/// need this to help the ROM bootloader get our code up and running.
#[link_section = ".boot2"]
#[used]
pub static BOOT2: [u8; 256] = rp2040_boot2::BOOT_LOADER_GENERIC_03H;

/// External high-speed crystal on the Raspberry Pi Pico board is 12 MHz.
/// Needs to be adjust if your board has a different frequency
const XTAL_FREQ_HZ: u32 = 12_000_000u32;

/// Entry point to our bare-metal application.
///
/// The `#[rp2040_hal::entry]` macro ensures the Cortex-M start-up code calls this function
/// as soon as all global variables and the spinlock are initialised.
///
/// The function configures the RP2040 peripherals, then toggles a GPIO pin in
/// an infinite loop. The GPIO pin is connected to an LED on the Raspberry Pi Pico
/// which will blink to the morse code of "I love you".
#[rp2040_hal::entry]
fn main() -> ! {

	// Obtain ownership of the peripheral registers of the RP2040 microcontroller
	// If Peripherals::take() returns None, then the unwrap() method will panic and terminate the program.
	let mut pac = pac::Peripherals::take().unwrap();
	
	// Obtain ownership of the core peripheral registers of the RP2040 microcontroller
	// Core peripherals include the NVIC (Nested Vectored Interrupt Controller), SysTick timer, etc.
	let core = pac::CorePeripherals::take().unwrap();
	
	// Set up the watchdog driver - needed by the clock setup code
	let mut watchdog = hal::Watchdog::new(pac.WATCHDOG);
	
	// Configure the clocks
	let clocks = hal::clocks::init_clocks_and_plls(
		XTAL_FREQ_HZ,
		pac.XOSC,
		pac.CLOCKS,
		pac.PLL_SYS,
		pac.PLL_USB,
		&mut pac.RESETS,
		&mut watchdog,
	)
	.ok()
	.unwrap();
	
	// Create a function called delay that is bound to the system timer and has a delay
	// resolution of one clock cycle, based on the system clock frequency.
	// core.SYST refers to the System Timer peripheral of the ARM Cortex-M microcontroller core.
	// clocks.system_clock.freq().to_Hz() returns the frequency of the system clock in hertz.
	let mut delay = cortex_m::delay::Delay::new(core.SYST, clocks.system_clock.freq().to_Hz());
	
	// The single-cycle I/O block controls our GPIO pins
	// pac = Peripheral Access Crate
	let sio = hal::Sio::new(pac.SIO);
	
	// Set the pins to their default state
	// Hal = Hardware Abstraction Layer
	// gpio = General Purpose Input/Output
	// Pins = The pins on the GPIO bank
	// new = Create a new instance of the GPIO bank
	let pins = hal::gpio::Pins::new(
		pac.IO_BANK0, // reference to the memory-mapped input/output (I/O) bank 0 peripheral of the Raspberry Pi Pico.
		pac.PADS_BANK0, // reference to the memory-mapped pad control register bank 0 peripheral of the Raspberry Pi Pico.
		sio.gpio_bank0, // reference to the SIO GPIO bank 0 peripheral of the Raspberry Pi Pico, which is used to configure and control the GPIO pins
		&mut pac.RESETS, // mutable reference to the reset control peripheral of the Raspberry Pi Pico.
	);
	
	// MAIN PROGRAM STARTS HERE

}

// End of file
```

# Devices on the Pico

- DHT11 sensor - Temperature and humidity sensor


# Pins

- Pin 39 (VSYS) - Used to supply power to the board from an external source, such as a battery or a power supply. This pin is connected directly to the input of the on-board voltage regulator, which regulates the input voltage to a stable 3.3V that is used to power the board's components. The maximum input voltage for the VSYS pin is 5.5V, and the minimum input voltage is 1.8V
- Pin 40 (VBUS) - Used to detect the presence of a USB host, such as a computer or a USB charger. When the board is connected to a USB host, the voltage on VBUS will be approximately 5V.