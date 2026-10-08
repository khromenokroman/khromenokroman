# Roman Khromenok

**C++ developer · network software · DPDK · Linux**

I build network software in C++ for Linux. At [T-Argos](https://t-argos.ru/) I work on a
next-generation firewall (NGFW) with a DPDK data plane and a Clixon (NETCONF/RESTCONF/YANG)
management plane: transceiver diagnostics, cluster session sync, DHCP relay, CPU isolation
for the data plane, kernel modules and configuration migration between YANG model versions.

I also contribute upstream to the projects I use at work.

## Open source contributions

**[DPDK](https://www.dpdk.org/)** (`lib/ethdev`, `app/testpmd`), accepted into dpdk-next-net:
- Public API to decode pluggable module EEPROM (SFP/QSFP): `rte_eth_module_eeprom_parse()`
- SFF-8472 fixes: undefined behavior in external calibration, Rx power calibration, rounding
- testpmd command `show port <id> module_eeprom decode`
- In review: SFF-8636 per-lane LOS / LOL / Tx fault flags, SFF-8472 Rx LOS and Tx fault state

[All DPDK patches](https://patches.dpdk.org/project/dpdk/list/?submitter=3957&state=*)

**[Clixon](https://github.com/clicon/clixon)** (NETCONF/RESTCONF/YANG):
- [Replace `select()` with `poll()` in the event loop](https://github.com/clicon/clixon/pull/584)
- [`cli_start_program()`: run external programs (Python, Bash) from the CLI](https://github.com/clicon/clixon/pull/522)
- In review: [XPath](https://github.com/clicon/clixon/pull/703) and
  [leafref](https://github.com/clicon/clixon/pull/700) validation fixes,
  [NACM group lookup helpers](https://github.com/clicon/clixon/pull/697)

**[CLIgen](https://github.com/clicon/cligen)**:
[version target in Makefile](https://github.com/clicon/cligen/pull/118), Debian packages built in CI

**[Eclipse iceoryx](https://github.com/eclipse-iceoryx/iceoryx)** (zero-copy IPC):
[`ssize_t` refactoring for Windows compatibility](https://github.com/eclipse-iceoryx/iceoryx/pull/2342)

## Selected projects

| Project | Description |
|---|---|
| [dpdk_informer](https://github.com/khromenokroman/dpdk_informer) | C++ library that reports DPDK port state, link, hardware properties and Rx/Tx statistics as JSON |
| [Interface_informer](https://github.com/khromenokroman/Interface_informer) | C++ library over netlink: interfaces, addresses, routes and ARP/NDP neighbors as JSON, network namespaces supported |
| [net-map](https://github.com/khromenokroman/net-map) | Service that discovers hosts in local subnets with ARP, detects IP conflicts and DHCP servers, shows a web map |
| [wifi-air](https://github.com/khromenokroman/wifi-air) | Terminal tool that shows Wi-Fi networks on air with the real occupied spectrum, to pick non-overlapping channels |
| [vpn](https://github.com/khromenokroman/vpn) | VPN tunnel over TUN and UDP with XChaCha20-Poly1305 encryption, plus an [Android client](https://github.com/khromenokroman/vpn_android) |
| [service-monitor](https://github.com/khromenokroman/service-monitor) | Web dashboard for systemd services over D-Bus and system resources from `/proc` and `/sys` |

## Stack

**Languages:** C++17/20, C, Python, Bash

**Networking:** DPDK, netlink, ethtool, TCP/IP, VLAN, LACP, OSPF, VPN, SFF-8472/8636 transceivers,
MikroTik (MTCNA), Cisco, Huawei

**Management plane:** NETCONF, RESTCONF, YANG, Clixon, CLIgen, D-Bus

**Linux:** kernel modules, io_uring, POSIX, systemd

**Libraries:** Boost.Asio, nlohmann/json, spdlog, fmt, sdbus-c++

**Tools:** CMake, GoogleTest, GitLab CI, Docker, Ansible, gdb, perf, Valgrind, tcpdump, Wireshark

## Contact

- Email: roma55592@yandex.ru
- Telegram: [@KhromenokRoman](https://t.me/KhromenokRoman)

<p><img src="https://github-readme-stats.vercel.app/api?username=khromenokroman&show_icons=true&locale=en" alt="GitHub stats" /></p>
