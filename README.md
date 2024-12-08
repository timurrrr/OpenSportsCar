# OpenSportsCar

## Mission
The mission of OpenSportsCar is to make performance data logging accessible for all sports cars.

## Philosophy

Data logging for performance-related parameters of sports cars has many applications. To name a few,
it unlocks opportunities for performance analysis of cars for high-performance driving education,
racing, as well as for third party when developing performance upgrades.

Some basic performance logging such as vehicle speed, RPM and accelerator pedal position can be
logged via a standardized protocol called OBD-II. Unfortunately, that protocol was intended for
_emissions control_, not performance, and thus it doesn't specify standard PIDs for other important
parameters, such as steering angle, brake pressure, fuel level, etc. Furthermore, as it uses a
request-response model, it also has limited data throughput, further limiting its usefulness for
performance analysis application.

Most modern cars use CAN bus (or multiple CAN buses) to facilitate communication between their ECUs.
If it's possible to connect a data logging device directly to the CAN bus, one can expect very high
refresh rates and completeness of data. Unfortunately, different car manufacturers (and even car
models from the same manufacturer) use different protocols to communicate data over the CAN bus, and
in most cases don't document those protocols anywhere. Furthermore, while older cars typically
exposed the CAN bus via the OBD-II port, many modern cars isolate the two from each other, which
means it's necessary to find alternative connection points for data loggers. Some car models have
hidden unused connector that have CAN data, ...

TODO: continue
