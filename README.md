# Packet Tracing Tool Using SDN

## Overview
This project implements a basic **Software Defined Networking (SDN) packet tracing tool** using the **Ryu controller** and a **Mininet custom topology**.  
Its purpose is to identify how packets move through the network, show forwarding behavior at switches, and help analyze packet paths in an SDN environment.

## Objective
The main objective of this project is to:

- identify and display the path taken by packets
- track forwarding behavior and flow installation
- identify forwarding paths between hosts
- display the route followed in the SDN topology
- validate the behavior using Mininet-based tests

## Project Structure
```bash
packet-tracing-tool-using-SDN/
├── path_tracer.py
├── topology.py
└── README.md
```

### File Description
- **path_tracer.py**: Ryu controller application for packet tracing and basic forwarding.
- **topology.py**: Mininet topology script defining a diamond topology.
- **README.md**: Project documentation.

## Technologies Used
- Python
- Ryu SDN Controller
- OpenFlow 1.3
- Mininet
- Ubuntu/Linux

## Topology Used
The project uses a **diamond topology**:

- `h1` connected to `s1`
- `s1` connected to `s2` and `s3`
- `s2` connected to `s4`
- `s3` connected to `s4`
- `s4` connected to `h2`

This topology is useful for demonstrating packet forwarding paths across multiple switches.

## How It Works
1. The controller installs a **table-miss flow rule** so unknown packets are sent to the controller.
2. The controller learns **MAC-to-port mappings** for each switch.
3. When a packet arrives:
   - if the destination is known, it forwards to the learned port
   - otherwise, it floods the packet
4. For **IPv4 packets**, the controller prints path trace information.
5. For **ARP packets**, the controller logs discovery events.
6. Known flows are installed into the switch to reduce repeated PacketIn events.

## path_tracer.py Highlights
The controller:
- uses **OpenFlow 1.3**
- handles **switch feature events**
- installs forwarding rules
- prints packet trace logs for IPv4 traffic
- tracks ARP discovery
- performs simple MAC learning switch behavior

## topology.py Highlights
The topology script:
- creates 4 switches (`s1`, `s2`, `s3`, `s4`)
- creates 2 hosts (`h1`, `h2`)
- connects them in a diamond shape
- can be loaded in Mininet for testing packet paths

## Prerequisites
Make sure the following are installed on Ubuntu:

- Python 3
- Mininet
- Ryu controller
- Open vSwitch

## Installation
Clone the repository:

```bash
git clone git@github.com:Deepikachettiar/packet-tracing-tool-using-SDN.git
cd packet-tracing-tool-using-SDN
```

## Running the Project

### 1. Start the Ryu controller
```bash
ryu-manager path_tracer.py
```

### 2. Run the Mininet topology
Open another terminal and run:

```bash
sudo mn --custom topology.py --topo diamond --controller remote
```

### 3. Test connectivity
Inside Mininet:

```bash
pingall
```

You can also test manually:

```bash
h1 ping h2
```

## Expected Output
When packets are sent, the controller prints logs similar to:

```text
[PATH TRACE] IPv4: <src-mac> -> Switch <dpid> (Port <in_port>) -> Out Port <out_port>
[PATH TRACE] ARP Discovery at Switch <dpid>
```

These logs help identify how packets are forwarded across the topology.

## Requirement Mapping

| Requirement | Current Status |
|---|---|
| Identify and display packet path | Partially implemented |
| Track flow rules | Basic implementation present |
| Identify forwarding path | Implemented at controller log level |
| Display route | Available as console output |
| Validate using tests | Can be tested using `pingall` and host pings |

## Limitations
- Route display is console-based only
- No GUI or graphical path visualization
- No separate flow table inspection module
- No automated test scripts included yet
- README in the original repository was incomplete

## Future Enhancements
- add end-to-end path reconstruction
- add graphical path visualization
- add flow rule inspection
- add automated test cases
- add log file export
- improve code formatting and comments

## Author
**Deepika K**


