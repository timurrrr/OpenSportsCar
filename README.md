# OpenSportsCar

## Mission
The mission of OpenSportsCar is to make performance data logging accessible for all sports cars.

## Philosophy

Data logging for low leve parameters of sports cars has many applications. To name a few, it unlocks
opportunities for performance analysis of cars for high-performance driving education (including
track days), entry-level competition such as autocross and time trials, as well as for third party
when developing performance upgrades.

On most modern cars, basic performance logging such as vehicle speed, RPM and accelerator pedal
position can be logged via a standardized protocol called OBD-II. Unfortunately, that protocol was
intended for _emissions control_, not performance, and thus it doesn't specify standard PIDs for
other important parameters, such as steering angle, brake pressure, fuel level, etc. Furthermore, as
it uses a request-response model, it also has limited data throughput, further limiting its
usefulness for performance analysis application.

Most modern cars use CAN bus (or multiple CAN buses) to facilitate communication between their ECUs.
If it's possible to connect a data logging device directly to the CAN bus, one can expect very high
refresh rates and completeness of data. Unfortunately, different car manufacturers (and even car
models from the same manufacturer) use different protocols to communicate data over the CAN bus, and
in most cases don't document those protocols anywhere. Furthermore, while older cars typically
exposed the CAN bus via the OBD-II port, many modern cars isolate the two from each other, which
means it's necessary to find alternative connection points for data loggers. Some car models have
hidden unused connectors that have CAN data, while other unfortunately require splicing wires.
Things get even worse on some cars where all the useful data isn't available on a single CAN bus,
and requires connecting to multiple CAN buses at the same time.

## OpenSportsCar support levels

Depending on the availability of data and complexity of logging, we propose the following "support
level" to categorize different cars:

### Level 1

To qualify for the support Level 1, a car should have its performance data available through either
the OBD-II port, or an easily accessible unused connector, or an easily accessed simple connector
for a non-safety related ECU that allows access to the data using a simple T-style wiring harness.

At least the following basic data channels must be available with at least the following update rates
to qualify for this level:

Data channel | Minimal update rate (Hz) | Comment
------------ | ------------------------ | -------
Speed                      | 20         |
Accelerator pedal position | 20         |
Steering wheel angle       | 20         |
Brake pressure             | 50         | ... or "brake pedal position"
RPM                        | 20         | For ICE cars
Engine oil temperature     | 1          | For ICE cars
Battery charge, %          | 1          | For EVs and hybrids
Battery power consumption  | 10         | For EVs and hybrids

Cars with outstanding logging capabilities, such as high update rates, detailed channels such as
tire pressures, IMU, etc. will be called out as "Level 1 ⭐".

### Level 2

TODO: continue
