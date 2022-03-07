# Syslog #
- ```show logging```
- ```logging 10.0.0.100``` Send syslog events to that server.
- ```logging trap debugging```
- Debug output is logged to the console line and buffer by default.
- Use the ```terminal monitor``` command in privileged mode to enable debug out to the VTY lines. Without it, ssh and telnet sessions are not going to see the debug output.