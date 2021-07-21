# ArduinoShell
An Arduino shell library for TCP/IP (Telnet) shell control.

Based on the terrific arduino-projects Shell: https://github.com/rweather/arduino-projects

My mods were to:

* Tweak `Shell` to work better when used as part of a C++ class; this mostly involved factoring out `ShellCommandInfo` to enable other classes to interface with commands. In particular, one of my projects has a touch screen and I wanted buttons to invoke the same commands defined by the shell.
* Made initialization work more naturally w/in a class member.
* Added a new macro that makes command initialization code cleaner, using a lambda (with no capture)

```c++

class Api : public Printable {
 public:
  ShellCommandRegister* cmdAbout = nullptr;
  ShellCommandRegister* cmdReveal = nullptr;

...

Api::Api() : currentCommand(nullptr), _macaddress{MAC_ADDRESS}, _ip(STATIC_IP), _shellServer(23) {
  // The ShellCommandClass(name, help, function), exapnds to this:
  cmdAbout = new ShellCommandRegister(F("about"), F("Prints version info"),
      [](Shell &shell, ShellCommandRegister *command, int argc, const ShellArguments &argv) {
        Api::getInstance().logCommand(command->name, argc, argv);
        shell.println("TV Slider");
      });

  cmdReveal = ShellCommandClass(reveal, "Reveals the TV", {
    Api::getInstance().logCommand(command->name, argc, argv);
    System::getInstance().setTrigger(System::Triggers::Reveal);
  });

  ...
```

## Includes
* Terminal class that extends Stream with functions suitable for interfacing to a VT100-compatible terminal program like PuTTY.
* Shell class that manages a Unix-like command-line shell for executing commands via a serial port or telnet session. Shell is built on top of the functionality of Terminal.
* LoginShell class that extends Shell to provide a simple username and password login mechanism.
* SerialShell example that shows how to use Shell to provide command-line access via a serial port.
* TelnetServer example that shows how to use LoginShell to provide command-line access via the telnet protocol.