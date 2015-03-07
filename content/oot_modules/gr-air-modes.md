---
title: gr-air-modes
type: application
contact :  {author: Nick Foster,
            email: bistromath@gmail.com}
website: https://github.com/bistromath/gr-air-modes
copyright_owner: Nick Foster
stable_release: master
gr_compat: {min: v3.5.0, max: v3.7.*}
dependencies: {UHD, NumPy, SciPy, OSMOSDR, SQLite}
brief: >
    gr-air-modes implements a software-defined radio receiver for Mode S
    transponder signals, including ADS-B reports from equipped aircraft.
repo: https://github.com/bistromath/gr-air-modes.git
---

Mode S is the transponder protocol used in modern commercial aircraft.
A Mode S-equipped aircraft replies to radar interrogation by either
ground radar (secondary surveillance) or other aircraft ("Traffic
Collision Avoidance System", or TCAS). The protocol is an extended
version of the Mode A/C protocol used in transponders since the 1940s.
Mode S reports include a unique airframe identifier (referred to
as the "ICAO number") and altitude (to facilitate separation control).
This receiver listens to the 1090MHz downlink channel; interrogation
requests at 1030MHz are not received or decoded by this program.

Automatic Dependent Surveillance-Broadcast (ADS-B) is a communication
protocol using the Extended Squitter capability of the Mode S transport
layer. There are other implementations (VDL Mode 2 and UAT, for
instance) but Mode S remains the primary ADS-B transport for commercial
use. The protocol is:

* Automatic: it requires no pilot input
* Dependent: it is dependent on altimeter, GPS, and other aircraft
  instrumentation for information
* Surveillance: it provides current information about the transmitting
  aircraft
* Broadcast: it is one-way, broadcast to all receivers within range.

ADS-B-equipped aircraft broadcast ("squitter") their position, velocity,
flight number, and other interesting information to any receiver within
range of the aircraft. Position reports are typically generated once per
second and flight indentification every five seconds.

Implementation of ADS-B is mandatory in European airspace as well as
in Australia. North American implementation is still voluntary, with
a mandate arriving in 2020 via the FAA's "NextGen" program.

The receiver modes_rx is written for use with Ettus Research USRP
devices, although the "RTLSDR" receivers are also supported via the
Osmocom driver. In theory, any receiver which outputs complex samples at
at least 2Msps should work via the file input or UDP input options, or
by means of a Gnuradio interface. Multiple output formats are supported:

* Raw (or minimally processed) output of packet data
* Parsed text
* SQLite database
* KML for use with Google Earth
* SBS-1-compatible output for use with e.g. PlanePlotter or Virtual
  Radar Server
* FlightGear multiplayer interface for real-time display of traffic
  within the simulator

Most of the common ADS-B reports are fully decoded per specification.
Those that are not are generally ones which are not commonly used.

Should you receive a large number of reports which result in
"not implemented" or "No handler" messages, please use the -w option to
save raw data and forward it to the author. To save time, note that
receiving a small number of spurious reports is expected; false reports
can be excluded by looking for multiple reports from the same aircraft
(i.e., the same ICAO 6-digit hexadecimal number).

