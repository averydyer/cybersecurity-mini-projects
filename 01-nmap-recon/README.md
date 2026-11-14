# Nmap Network Reconnaissance

## Project Objective

The aim of this project was to learn how to investigate ports using Nmap by checking to see whether ports are open or closed and identifying processes listening on TCP ports through IPv4 and IPv6 sockets. 

Although an open port is not necessarily remotely accessible, identifying open ports helps security analysts understand a system's potential entry points. It is also important to understand the listening configurations of services because IPv4 and IPv6 can have different network accessibility.

## Tools Used

- **Nmap:** Used to scan the 1,000 most common TCP ports over IPv4 and perform targeted scans of ports 5000 and 7000 over IPv6.
- **lsof:** Used to identify which processes were listening on open TCP ports.
- **Homebrew:** Used to install Nmap as a command-line tool on macOS

## Methodology

### Initial TCP Port Scan

``` bash
nmap -sT 127.0.0.1
```

This command uses Nmap to perform a TCP connect scan on the 1,000 most common ports. A TCP connect scan attempts to establish a connection with each port to determine its state. The loopback address (27.0.0.1) was used to ensure the investigation stayed local. 

The scan identified 998 closed ports and two open TCP ports (5000 and 7000) over IPv4.

### IPv6 TCP Port Scan

``` bash
nmap -6 -sT -p 5000,7000 ::1
```

This command uses Nmap to perform another TCP connect scan, this time using IPv6 (-6) instead of IPv4 and attempting to establish a connection with only ports 5000 and 7000 (-p 5000,7000). A loopback address (::1) is also used here.

The scan identified both TCP ports 5000 and 7000 as being open over IPv6.

### Listening Process Investigation

``` bash
lsof -nP -iTCP:5000 -sTCP:LISTEN
lsof -nP -iTCP:7000 -sTCP:LISTEN
```

lsof was used to investigate which applications were listening on ports 5000 and 7000. Although Nmap provided service labels on the open ports, these labels do not necessarily identify the applications actually listening on those ports.

The results identified a process named ControlCe (PID 1301) listening on both ports, with separate IPv4 and IPv6 sockets. This process is likely macOS Control Center, which may be associated with AirPlay functionality.

## Security Findings and Lessons Learned

The investigation identified two open TCP ports, 5000 and 7000, associated with a process likely belonging to macOS ControlCenter. While these findings do not necessarily indicate a security vulnerability, additional investigation would be needed to determine whether the ports are accessible from other devices and whether the services present any security risks. Because the scans targeted loopback addresses, they do not establish the services' accessibility over the network.

Before starting this project, I was unfamiliar with IPv4 and IPv6 and how services can have different network accessibility depending on the IP version being used. I now have a better understanding of what it means for a port to be open or closed, how applications can listen on multiple ports simultaneously, and how Nmap and lsof can be used together to investigate network services.