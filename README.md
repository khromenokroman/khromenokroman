<p align="center">
 <img width="600" src="snake.svg" alt="snake"/>
</p>

# Roman Khromenok

**C++ developer · network software · DPDK · Linux**

I build network software in C++ for Linux. At [T-Argos](https://t-argos.ru/) I work on a
next-generation firewall (NGFW) with a DPDK data plane and a Clixon (NETCONF/RESTCONF/YANG)
management plane: transceiver diagnostics, cluster session sync, DHCP relay, CPU isolation
for the data plane, kernel modules and configuration migration between YANG model versions.

I also contribute upstream to the projects I use at work.

## Open source contributions

**[systemd](https://github.com/systemd/systemd)** (`systemd-networkd`, `sd-dhcp-client`, `sd-dhcp6-client`), merged:
- [networkd: follow system hostname changes in DHCP clients](https://github.com/systemd/systemd/pull/44116):
  the new hostname is sent to DHCPv4/DHCPv6 servers without reconfiguring the interface, the DHCPv4
  lease is renewed immediately, new `SendHostnameUpdates=` option
  (fixes [#36440](https://github.com/systemd/systemd/issues/36440),
  [#36792](https://github.com/systemd/systemd/issues/36792))
- [networkd: renew DHCPv6 lease on hostname change and on `networkctl renew`](https://github.com/systemd/systemd/pull/44161):
  new `sd_dhcp6_client_send_renew()`, the new hostname reaches DHCPv6 servers immediately,
  `networkctl renew` now renews the DHCPv6 lease too
- [networkd: request DHCPv6 information again on `networkctl renew`](https://github.com/systemd/systemd/pull/44352):
  stateless DHCPv6 (Information-Request) clients refresh DNS/NTP servers on demand
  instead of waiting for the information refresh time (one day by default)
- [networkd: show why a link is still in the configuring state](https://github.com/systemd/systemd/pull/44168):
  new `ConfiguringReasons` field in the Varlink `Describe` reply, shown by `networkctl status`
  (fixes [#18222](https://github.com/systemd/systemd/issues/18222))
- [networkd: enable IPv6 link-local addressing on WireGuard interfaces](https://github.com/systemd/systemd/pull/44367):
  the address generation mode reset by the WireGuard driver on every link up is restored, so routing
  protocols like Babel work over WireGuard out of the box
  (fixes [#44005](https://github.com/systemd/systemd/issues/44005))
- [DHCP server: do not refuse a host with a static lease whose client ID changed](https://github.com/systemd/systemd/pull/44188):
  the host gets its static address again instead of being ignored until the old lease expires
  (fixes [#44120](https://github.com/systemd/systemd/issues/44120))

**[DPDK](https://www.dpdk.org/)** (`lib/ethdev`, `app/testpmd`), 18 patches accepted into dpdk-next-net for DPDK 26.11:
- Public API to decode pluggable module EEPROM (SFP/QSFP): `rte_eth_module_eeprom_parse()`
- testpmd command `show port <id> module_eeprom decode`
- SFF-8636 per-lane LOS / LOL / Tx fault flags, SFF-8472 Rx LOS and Tx fault state
- Newer SFF compliance codes: Fibre Channel 1600/3200/6400 MBytes/sec, SAS 12G/24G
- Decoder fixes: out-of-bounds read of SFF-8636 thresholds, unaligned access, undefined behavior
  in SFF-8472 external calibration, Rx power calibration and rounding
- In review: RFC to move the decoders into a new `lib/transceiver` library (to be maintained by me),
  module EEPROM decoding in `dpdk-proc-info`

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
