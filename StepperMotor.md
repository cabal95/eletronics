# Hardware

These are the devices I have used and ordered in working with stepper motors.

* https://www.amazon.com/gp/product/B00PQ4Y9B6
* https://www.amazon.com/gp/product/B075XH1TSJ

# Wiring

Wiring from the DRV8825 is as follows.

## Power Supply Connections

The `SLP` and `RST` pins are both connected to the I/O board (raspberry pi) and use the +3.3v feed. The GND pin opposite the `DIR` pin is connected to the GND of the I/O board.

`VMOT` and the `GND` pin next to it connect to your motor power source. Thus far I have used a 12v source and connect `VMOT` to +12v and the `GND` to -12v. For the record, on all the DRV8825 boards I've played with, this always looks like **U** MOT and not **V** MOT.

## Motor Connections

There are four wires on the bipolor motor. All four of these connect to the DRV8825 board as follows.

* `2B` pin on DRV8825 connects to the Blue wire on motor, labelled on motor sheet as `B\` which appears to mean `B-`.
* `1B` pin on DRV8825 connects to the Red wire on motor (this looks more magenta to me, but it's labelled red), labelled on motor sheet as `B` which appears to mean `B+`.
* `1A` pin on DRV8825 connects to the Black wire on motor, labelled on motor sheet as `A` which appears to mean `A+`
* `1B` pin on DRV8825 connects to the Green wire on motor, labelled on motor sheet as `A\` which appears to mean `A-`.

There will be a constant high pitched whine. This is somewhat normal, though there is some tinkering that can probably be done to minimize it. The reason for it seems to be that the two coils that move the motor are always energized. My original understanding was that they were only energized while the motor moved but that is not the case. The motor remains energized at idle to hold in place. You can see this by trying to manually turn the rod while power is applied. This also means it's probably a good idea to _not_ leave it powered on 24/7 as it's going to always be using power and generating heat.

## Control Connections

Thus far I have only used the `STP` pin. When it transitions from Low to High (that is from not connected to the same +3.3v source connected to the `SLP` and `RST` pins) it will move one unit of measurement. That unit of measurement is controlled by the `M0`, `M1`, and `M2` pins. I have not played with those yet.

Without the **M** pins connected the unit operates in full-step mode. This means each tick of the `STP` pin will move an entire step. On this particular motor, that means 1.8 degrees. Or said another way, 1/200th of a full revolution. The DRV8825 supports going down to 1/32nd microsteps, which theoretically means you get up to 6,400 microsteps per revolution.

When left idle, the motor gets HOT. Like, maybe burn whatever it is sitting on hot. To that end, it is highly recommended to connect the `EN` (enable) pin and use it to turn on/off the motor at various times to prevent overheating (really, to prevent from burning yourself). Especially when doing debugging, you don't want to have the motor holding position for 45 minutes while you debug some code before your next test.
