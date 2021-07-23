# ArduinoShell
An Arduino library for TCP/IP (Telnet) shell control

## Building Samples with PlatformIO

`pio ci --lib="src" --project-option="lib_deps=arduino-libraries/Ethernet@^2.0.0" --board=megaatmega2560 .\examples\Telnetserver\TelnetServer.ino`