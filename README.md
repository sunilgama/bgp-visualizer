# BGP Visualizer

Interactive, animated explanations of the Border Gateway Protocol (BGP), built for people learning networking from RFCs, vendor documentation and packet captures. Watch each packet, timer and state change one step at a time, and click any packet to see its fields.

There are two independent modules. Each is a single HTML file with no libraries, no build step and no network access needed. Open it in a browser and it works.

| Module | File | Focus |
|--------|------|-------|
| Basics | `bgp-visualizer.html` | BGP-4 as defined in RFC 4271: session setup, the finite state machine, UPDATE, timers, errors |
| Advanced | `bgp-advanced.html` | Route reflectors, confederations, loop prevention, path selection, flap damping, MPLS L3VPN, EVPN |

**Live demo:** https://YOUR-USERNAME.github.io/bgp-visualizer/bgp-visualizer.html and https://YOUR-USERNAME.github.io/bgp-visualizer/bgp-advanced.html

## Module 1: BGP basics (`bgp-visualizer.html`)

Five scenarios, each shown as packets moving between routers, with the finite state machine (Idle, Connect, Active, OpenSent, OpenConfirm, Established), timers and routing tables updating live.

1. **Session setup:** TCP three-way handshake on port 179, OPEN, timer negotiation, KEEPALIVE, first UPDATE, keepalive cycle.
2. **Route advertisement:** an UPDATE crossing three ASes, best-path installation, AS_PATH loop rejection, route withdrawal.
3. **Hold timer expiry:** a silent link failure, the Hold timer reaching zero, NOTIFICATION, withdrawal toward other neighbours, and recovery.
4. **OPEN message error:** a wrong remote AS gives NOTIFICATION code 2, subcode 2 (Bad Peer AS), then the retry loop.
5. **TCP failure and retry:** lost SYNs, TCP retransmission, RST, the Connect to Active transition and the ConnectRetryTimer.

## Module 2: Advanced concepts (`bgp-advanced.html`)

Seven scenarios, 88 steps in total, each with failure cases and packet-field detail.

1. **Route reflectors (RFC 4456):** reflection rules, ORIGINATOR_ID and CLUSTER_LIST, path hiding, redundant reflectors, reflector failure.
2. **Confederations (RFC 5065):** member ASes, AS_CONFED_SEQUENCE, the two AS numbers in the OPEN, attribute handling at each boundary, a Bad Peer AS failure.
3. **AS_PATH loop prevention:** the eBGP rule, prepending, AS_SET after aggregation, loop rules for each design, and legitimate repeated-AS cases.
4. **Path selection:** Weight, LOCAL_PREF, AS_PATH, ORIGIN, MED, eBGP over iBGP, IGP cost and router ID, shown as a candidate table that narrows step by step.
5. **Route flap damping (RFC 2439, RFC 7196):** a live penalty chart with suppress and reuse limits, and the decay arithmetic.
6. **MPLS L3VPN (RFC 4364):** VRFs, MP-BGP VPNv4, Route Distinguishers, Route Targets, LDP transport labels, the two-label forwarding path, an RT mismatch failure, RT design patterns.
7. **EVPN route types (RFC 7432, RFC 9136):** types 1 to 5, ARP suppression, BUM replication, MAC mobility, multi-homing and designated forwarder election, symmetric IRB.

## Controls

- **Play, Pause, Next step, Previous, Reset** and a speed slider.
- Click a packet in the diagram (or in the packet log) to open the packet inspector with its headers and fields.
- Left and right arrow keys move between steps.
- Click a step in the list to jump to it.
- Colour key: grey is control (TCP), blue is a request or advertisement (OPEN, UPDATE), green is a response (KEEPALIVE), red is an error, purple is user data.

## Run locally

Download the HTML file and open it in a browser. Nothing else is required, and it works offline.

## Assumptions and simplifications

- The basics module follows RFC 4271. Extensions are not animated: multiprotocol BGP, 4-byte AS numbers, route refresh, graceful restart.
- OPEN messages are drawn without optional parameters. Real routers add capabilities, so real OPEN messages are longer.
- The optional DelayOpen and IdleHold timers, connection collision handling and route flap damping at the session level are not shown in the basics module.
- Weight is a vendor feature and is not part of RFC 4271. The damping parameters (1000 per flap, 15 minute half-life, suppress above 2000, reuse below 750) are common vendor defaults, not RFC values.
- Time-lapse steps compress seconds or minutes so you can watch timers move. TCP sequence numbers are shown only on handshake segments.
- The route selection in the basics module uses a single candidate. The full process is in the advanced module and in RFC 4271 section 9.1.
- All AS numbers, addresses, labels and VNIs are examples. The inspector shows typical values, not captured data.
- Use a packet capture or the real device to confirm behaviour on a specific platform, because vendors differ in detail.

## Publish with GitHub Pages

1. Create a public repository, for example `bgp-visualizer`.
2. Upload `bgp-visualizer.html`, `bgp-advanced.html`, `README.md` and `LICENSE`.
3. Go to **Settings, then Pages**. Choose **Deploy from a branch**, branch `main`, folder `/ (root)`.
4. After a minute or two each page is live at `https://YOUR-USERNAME.github.io/bgp-visualizer/<file name>`.
5. Replace `YOUR-USERNAME` in this README with your GitHub username.

## References

- [RFC 4271](https://www.rfc-editor.org/rfc/rfc4271): BGP-4
- [RFC 4456](https://www.rfc-editor.org/rfc/rfc4456): BGP route reflection
- [RFC 5065](https://www.rfc-editor.org/rfc/rfc5065): Autonomous system confederations for BGP
- [RFC 2439](https://www.rfc-editor.org/rfc/rfc2439): BGP route flap damping
- [RFC 7196](https://www.rfc-editor.org/rfc/rfc7196): Making route flap damping usable
- [RFC 4760](https://www.rfc-editor.org/rfc/rfc4760): Multiprotocol extensions for BGP-4
- [RFC 4364](https://www.rfc-editor.org/rfc/rfc4364): BGP/MPLS IP virtual private networks
- [RFC 7432](https://www.rfc-editor.org/rfc/rfc7432): BGP MPLS-based Ethernet VPN
- [RFC 9136](https://www.rfc-editor.org/rfc/rfc9136): IP prefix advertisement in EVPN
- [RFC 9135](https://www.rfc-editor.org/rfc/rfc9135): Integrated routing and bridging in EVPN
- [RFC 7348](https://www.rfc-editor.org/rfc/rfc7348): VXLAN
- [RFC 5036](https://www.rfc-editor.org/rfc/rfc5036): LDP specification
- [RFC 3031](https://www.rfc-editor.org/rfc/rfc3031): MPLS architecture

## License

MIT. See the `LICENSE` file. Replace `YOUR NAME` in it with your name before publishing.
