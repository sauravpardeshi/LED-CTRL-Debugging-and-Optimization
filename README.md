# LED-CTRL: Debugging and Optimization of an Embedded Firmware

Embedded systems internship task: debugging techniques and optimization strategies.

## What this project is
LED-CTRL is an interrupt-driven firmware (C99) with a 1 kHz timer interrupt,
a UART command console, an ADC temperature alert and a watchdog. It runs on a
software model of the board, so no hardware is needed.

Eight realistic bugs were deliberately injected into a copy of the code (v0.9)
to simulate real-world faults. They were then found, fixed and the code was
optimized (v1.1.0).

## Results
- 9 defects found and fixed (8 injected + 1 latent)
- Tools used: AddressSanitizer, UBSan, gdb, valgrind, cppcheck, fuzzing, trace logging
- Test suite grown from 97 to 116 checks; all 8 bugs are now detected
- Cortex-M4 flash: 8,893 B -> 3,224 B (-64 %)
- Largest stack frame: 216 B -> 32 B
- Instructions per command: TEMP about 4.3x fewer, STATUS about 2.3x fewer
- Behavior verified bit-identical to the pre-optimization code
  (8 random scenarios x 300 simulated seconds)
- One optimization was measured, found to be worse, and rejected

## Files
- docs/LED-CTRL_Debugging_and_Optimization_Report.pdf : full report
- src/led_controller_v0.9_buggy.c : faulty version used for debugging
- src/led_controller_v1.1.0_optimized.c : fixed and optimized version with tests

## Build and run
    gcc -std=c99 -Wall -Wextra -Wpedantic -Werror -O2 src/led_controller_v1.1.0_optimized.c -o ledctl
    ./ledctl --selftest        # 116 checks, exit code 0 = all pass
    ./ledctl --demo            # scripted demo
    ./ledctl --interactive     # type UART commands yourself

To see the bugs, run the same commands on the v0.9 file:
    gcc -std=c99 -O2 src/led_controller_v0.9_buggy.c -o ledctl_buggy
    ./ledctl_buggy --selftest  # many checks fail

## Author
Saurav Pardeshi, BTech Electronics and Telecommunication
