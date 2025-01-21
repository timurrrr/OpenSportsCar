# OpenSportsCar

## Mission
The mission of OpenSportsCar is to make performance data logging accessible for all sports cars.

## Philosophy

Data logging for low-level parameters of sports cars has many applications. To name a few, it
unlocks opportunities for performance analysis of cars for high-performance driving education
(including track days), entry-level competition such as autocross and time trials, as well as for
third parties when developing performance upgrades.

On most modern cars, basic performance logging such as vehicle speed, RPM, and accelerator pedal
position can be logged via a standardized protocol called OBD-II. Unfortunately, that protocol was
intended for _emissions control_, not performance, and thus, it doesn't specify standard PIDs for
other important parameters, such as steering angle, brake pressure, fuel level, etc. Furthermore, as
it uses a request-response model, it also has limited data throughput, further limiting its
usefulness for performance analysis applications.

Most modern cars use the CAN bus (or multiple CAN buses) to facilitate communication between their
ECUs. If it's possible to connect a data logging device directly to the CAN bus, one can expect very
high refresh rates and completeness of data. Unfortunately, different car manufacturers (and even
car models from the same manufacturer) use different protocols to communicate data over the CAN bus
and, in most cases, don't document those protocols anywhere. Furthermore, while older cars typically
expose the CAN bus via the OBD-II port, many modern cars isolate the two from each other, which
means it's necessary to find alternative connection points for data loggers. Some car models have
hidden unused connectors providing access to the CAN bus, while others, unfortunately, require
splicing wires. Things get even worse on some cars where all the useful data isn't available on a
single CAN bus, which requires connecting to multiple CAN buses.

## OpenSportsCar tiers

Depending on the availability of data and the complexity of setting up logging, we propose the
following "data availability tiers" to categorize different cars:

### Tier 1

To qualify for Tier 1, a car should have its performance data available through either the OBD-II
port, an easily accessible unused connector
([example](https://github.com/timurrrr/ft86/blob/main/can_bus/gen2.md#dcm-connector)),
or an easily accessible simple connector for a non-safety related ECU that allows access to the data
using a simple T-style wiring harness (i.e. commonly available connectors, at most five wires in the
harness).

At least the following basic channels must be available with at least the following update rates to
qualify for this tier:

Data channel | Minimal update rate (Hz) | Comment
------------ | ------------------------ | -------
Speed                      | 20         |
Accelerator pedal position | 20         |
Steering wheel angle       | 20         |
Brake pressure             | 50         | or "brake pedal position"
RPM                        | 20         | For ICE (Internal Combustion Engine) cars
Engine oil temperature     | 1          | For ICE cars
Engine coolant temperature | 1          | For ICE cars
Battery charge, %          | 1          | For EVs and hybrids
Battery power consumption  | 10         | For EVs and hybrids

Note that all these data channels should be available with those update rates (or higher) _at the
same time_. While the OBD-II protocol on some cars can provide a ~20 Hz update rate for _one_
channel, in order to achieve Tier 1, the car needs to be able to provide all data at once.

Cars with outstanding logging capabilities, such as higher update rates or many additional data
channels available, will be called out as "Tier 1 ⭐".

Here's a list of other useful data channels recommended whenever possible:

Data channel | Minimal update rate (Hz) | Comment
------------ | ------------------------ | -------
Ambient air temperature    | 1          |
Lateral and longitudinal accelerations | 50 |
Yaw rate                   | 50         |
Torque demand              | 20         | For ICE cars
Intake air temperature     | 5          | For ICE cars
Throttle valve position    | 20         | For ICE cars
Intake manifold pressure   | 20         | For ICE cars
Lambda                     | 20         | For ICE cars
Timing advance/retardation | 20         | For ICE cars
Valve timing               | 20         | For ICE cars
Valve lift                 | 20         | For ICE cars
Engine oil pressure        | 20         | For ICE cars
Fuel level                 | 1          | For ICE cars
Gear                       | 20         | For ICE cars
Individual wheel speeds    | 50         |
Individual tire pressures  | 1          |

Other relevant data channels are welcome as well!

### Tier 2

Cars that don't qualify for Tier 1 can qualify for Tier 2 if they provide at least half the minimal
update rate required for Tier 1.

In addition to the connection options required for Tier 1, it's also acceptable to use more
complicated T-style wiring harnesses or to splice wiring going between non-safety ECUs.

### Tier 3

Tier 3 allows splicing wiring between safety-related ECUs.

## Suggestions to car manufacturers

### Hardware suggestions

Providing the performance-related CAN data through the OBD-II port is the best option, as many
performance tools (e.g., AIM Solo DL, OBDLink Bluetooth dongles) already available on the market
provide plug-and-play connectivity at the lowest possible cost to the user.

While it's understandable that car manufacturers want/need to isolate the internal data networks of
their cars from data sent by potentially malicious devices plugged into the OBD-II port, this
doesn't prevent _relaying_ pre-selected internal data from the internal CAN networks to the OBD-II
port. If there are concerns over undesired access of OBD-II devices to the underlying data (e.g., an
OBD-II dongle provided by an insurance company shouldn't monitor the RPM?), the user can be given
control over whether the CAN data is relayed to the OBD-II by using a toggle in the Settings menu
of the infotainment system.

This option should be possible to retrofit to some existing car models over a software update, but
it's understandable that in other cars this is either infeasible or too complicated to do at scale.

If providing data through the OBD-II port is not an option, there should be an easily accessible
connector port ([example](https://github.com/timurrrr/ft86/blob/main/can_bus/gen2.md#dcm-connector))
with the CAN bus, 12V power, and Ground available to the user. As car manufacturers have the best
understanding of the CAN bus of their cars, they can make a performance part such as a T-style
wiring harness that can be installed to provide such a port on an existing car. If desired, this
port can also be equipped with a simple CAN relay so that data can only be _read_ from the port,
but anything sent over this port into the car is ignored.

### User education suggestions

Besides simply exposing the data electronically, car enthusiasts also need to be able to interpret
that data correctly.
[Here](https://github.com/timurrrr/RaceChronoDiyBleDevice/blob/master/can_db/mazda_mx5_nd.md)
is an example of documentation on how to interpret data available on the CAN bus of a Mazda MX-5.

Unfortunately, the status quo is that data encoding for every car model has to be
reverse-engineered, which involves a lot of effort and introduces inaccuracies due to the amount of
guesswork involved. On top of that, it's hard to effectively spread such inherently incomplete and
occasionally changing knowledge to other car enthusiasts.

Instead, makers of sports cars should document the best way to connect third-party performance data
monitors/loggers and how to decode the data.

## Data availability tiers table

Below is a table that specifies the tiers that some car models qualify for. Note that any of these
cars can be promoted to a better tier as more information about them is discovered or in case of
software and/or hardware updates.

Car model  | Model years | Tier | Links | Notes
---------- | ----------- | ---- | ----- | -----
BMW F-chassis |          | Tier 3 |     | TODO: add details
Mazda MX-5 | 2005-2015 (NC) | Tier 1 ⭐ | [1](https://github.com/timurrrr/RaceChronoDiyBleDevice/blob/master/can_db/mazda_mx5_nc.md) | Abundance of data available over the OBD-II port
Mazda MX-5 | 2016-2023 (ND) | Tier | [1](https://github.com/timurrrr/RaceChronoDiyBleDevice/blob/master/can_db/mazda_mx5_nd.md) | Abundance of data available over the OBD-II port. ND3 (2024+) is not tiered yet.
Modern Porsches |          | Tier 3 |     | TODO: add details
Scion FR-S | 2013-2016   | Tier 1 ⭐ | [1](https://github.com/timurrrr/RaceChronoDiyBleDevice/blob/master/can_db/ft86.md) | Abundance of data available over the OBD-II port
Subaru BRZ | 2013-2020   | Tier 1 ⭐ | [1](https://github.com/timurrrr/RaceChronoDiyBleDevice/blob/master/can_db/ft86.md) | Abundance of data available over the OBD-II port
Subaru BRZ | 2022+       | Tier 2 | [1](https://github.com/timurrrr/RaceChronoDiyBleDevice/blob/master/can_db/ft86_gen2.md) | Almost Level 1, but requires a somewhat non-trivial T-style harness
Toyota 86/GT86 | 2013-2020 | Tier 1 ⭐ | [1](https://github.com/timurrrr/RaceChronoDiyBleDevice/blob/master/can_db/ft86.md) | Abundance of data available over the OBD-II port
Toyota GR86 | 2022+      | Tier 1 ⭐ | [1](https://github.com/timurrrr/RaceChronoDiyBleDevice/blob/master/can_db/ft86_gen2.md) | Abundance of data available over the unused connector in the glovebox
