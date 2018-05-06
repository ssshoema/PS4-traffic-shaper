Initially based on wondershaper 1.4 (The recent wondershaper version)

Goal 1 - Reduce gaming bufferbloat, prioritize PS4 console, and port priority for Destiny 2.
Goal 2 - Not to block or "severely" minimalize traffic for the rest of LAN.

Using tc qdisc to shape the ingress and egress of network with prioritization on the PS4 gaming system and Destiny 2 game with port, TOS, ICMP, and <64 byte packet prioritization.

There are multiple methods available for network shaping so a google search may yield better solutions for traffic shaping in your case. This is not intended as an all encompassing solution. Mileage may vary for multiple reasons.

Setup for this implementation is a Linux gateway PC with 2 NIC's one facing the internet and the other facing my LAN.
This shaping script is used on the internet facing NIC.

HTB/SFQ shaping in this instance was stable and was easily editable for fine tuning of classes, ports, and ingress/egress traffic. Again, there are multiple shaping algorithm possibilities which may provide better results.

Most scripts I studied were schemed towards a high priority, rest of traffic priority (medium), and low priority(non-desired traffic). This script classes are designed to not "squelch" any traffic but to tier traffic in such that my PS4, along with other high priority ip traffic that impacts gaming, is prioritized but all remaining network traffic does not nor is intended to suffer severely from the higher prioritization while the PS4 is in use (unless you want it to).

Port prioritization for PS4/Destiny 2 traffic is based on online port documentation and culling port information from IPTRAF during game play for the PS4 IP.

Tuning of the script will depend on multiple variables based on your ISP connection. 
Find your own MTU (QUANTUM) value and edit if needed.
Use the dslreports speed test which includes bufferbloat grade. http://www.dslreports.com/speedtest
Initiate the script with sudo ./PS4shape -a eth# -d numeric -u numeric where eth# is your WAN card, -d is 85 to 95% of ISP plan download speed and -u is 85-95% of ISP plan upload speed of the test.
My initial test on a 2Mbps up and 20Mbps down = Download 24.7 Upload 2.4 Grade C D A.
Test with script initiated sudo ./PS4shape -a eth3 -d 19500 -u 1800 = Download 19.2 Upload 1.8 Grade A+ A A+.
Clear the qdiscs and edit  -d -u numbers to fine tune if needed.
Tuning of traffic classes is pretty straight forward in the CLASS UPLINK section for rate and ceiling.
If you are a novice and plan on tuning in the classes recommended reading is advised. Google tc qdisc htb.
