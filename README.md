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

**[systemd](https://github.com/systemd/systemd)** (`systemd-networkd`, `sd-dhcp-client`, `sd-dhcp6-client`, `systemd-userdbd`), merged:
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
- [DHCP server: static leases by client identifier](https://github.com/systemd/systemd/pull/44409):
  new `ClientIdentifier=`, `IAID=`, `DUIDType=` and `DUIDRawData=` options in `[DHCPServerStaticLease]`,
  so hosts that share a hardware address (e.g. ipvlan in L2 mode) get their own static addresses
- [userdbd: stop respawning workers when the worker binary is broken](https://github.com/systemd/systemd/pull/44516):
  new `execvp_safe()` that does not run a truncated binary as an empty shell script via `/bin/sh`;
  `systemd-userdbd`, `systemd-mountfsd` and `systemd-nsresourced` exit with an error instead of
  respawning workers in a loop
  (fixes [#44280](https://github.com/systemd/systemd/issues/44280))
- [tree-wide: use `execvp_safe()` instead of `execvp()`, `execvpe()` and `execlp()`](https://github.com/systemd/systemd/pull/44543):
  all remaining callers converted, so a truncated binary is never run as an empty shell script via `/bin/sh`;
  scripts without a shebang (e.g. via `$SYSTEMD_EDITOR`, `$PAGER` or `systemd-run`) are no longer executed

**[DPDK](https://www.dpdk.org/)** (`lib/ethdev`, `app/testpmd`, `app/proc-info`), 19 patches merged into DPDK main for the 26.11 release:
- Public API to decode pluggable module EEPROM (SFP/QSFP): `rte_eth_module_eeprom_parse()`
- testpmd command `show port <id> module_eeprom decode`
- SFF-8636 per-lane LOS / LOL / Tx fault flags, SFF-8472 Rx LOS and Tx fault state
- Newer SFF compliance codes: Fibre Channel 1600/3200/6400 MBytes/sec, SAS 12G/24G
- Decoder fixes: out-of-bounds read of SFF-8636 thresholds, unaligned access, undefined behavior
  in SFF-8472 external calibration, Rx power calibration and rounding
  (the out-of-bounds read and SFF-8472 calibration fixes are backported to the 24.11 LTS branch, 24.11.9)
- `dpdk-proc-info --show-module-eeprom`: one port without a module no longer hides the modules
  of the next ports, EEPROM length reported by the driver is checked against the buffer
- In review: new `lib/transceiver` library for the decoders (to be maintained by me),
  module EEPROM decoding in `dpdk-proc-info`

[All DPDK patches](https://patches.dpdk.org/project/dpdk/list/?submitter=3957&state=*)

**[FRRouting](https://github.com/FRRouting/frr)** (`bgpd`), merged:
- [bgpd: apply `neighbor soo` to peer-group members](https://github.com/FRRouting/frr/pull/24210):
  Site-of-Origin set on a peer-group now reaches peers that are already members of the group,
  not only peers that join it later; covered by a new case in the `bgp_soo` topotest
  (fixes [#24209](https://github.com/FRRouting/frr/issues/24209))

**[Clixon](https://github.com/clicon/clixon)** (NETCONF/RESTCONF/YANG):
- [Replace `select()` with `poll()` in the event loop](https://github.com/clicon/clixon/pull/584)
- [`cli_start_program()`: run external programs (Python, Bash) from the CLI](https://github.com/clicon/clixon/pull/522)
- [`clixon_cli -e`: exit on the first failed command](https://github.com/clicon/clixon/pull/709):
  scripts run from stdin or a file (`-F`) stop at the first command that fails to parse or whose
  validate/commit fails, and exit with an error instead of applying a partial configuration
- [XPath `mod` with a zero divisor no longer crashes (SIGFPE); `xml_diff()` handles an empty first tree](https://github.com/clicon/clixon/pull/703)
- In review: [leafref validation fixes](https://github.com/clicon/clixon/pull/700),
  [NACM group lookup helpers](https://github.com/clicon/clixon/pull/697)

**[CLIgen](https://github.com/clicon/cligen)**:
[version target in Makefile](https://github.com/clicon/cligen/pull/118), Debian packages built in CI

**[sdbus-c++](https://github.com/Kistler-Group/sdbus-cpp)** (C++ D-Bus library):
- [`sdbus::Track` for tracking D-Bus peers](https://github.com/Kistler-Group/sdbus-cpp/pull/551):
  a wrapper over `sd_bus_track`, a service gets notified once its clients disconnect from the bus
  and releases resources held on their behalf (locks, sessions, subscriptions)
  (fixes [#57](https://github.com/Kistler-Group/sdbus-cpp/issues/57))
- [Valid C++ identifiers for argument names in the `xml2cpp` code generator](https://github.com/Kistler-Group/sdbus-cpp/pull/556),
  with compile tests for the generated bindings
  (fixes [#550](https://github.com/Kistler-Group/sdbus-cpp/issues/550))
- [Convenience API for watching property changes](https://github.com/Kistler-Group/sdbus-cpp/pull/557):
  `IProxy::uponPropertyChange()` subscribes to `PropertiesChanged` for a single property
  and delivers its typed value, or `std::optional<T>` for invalidated properties
  (fixes [#500](https://github.com/Kistler-Group/sdbus-cpp/issues/500))
- [Typed property value in the `getPropertyAsync()` callback](https://github.com/Kistler-Group/sdbus-cpp/pull/559):
  the reply handler may take the property value as `T` instead of `sdbus::Variant`,
  type mismatches are reported through the `std::optional<Error>` parameter
  (fixes [#552](https://github.com/Kistler-Group/sdbus-cpp/issues/552))

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
