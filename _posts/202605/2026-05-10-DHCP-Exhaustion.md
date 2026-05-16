---    
title:  " DHCP Exhaustion and Rogue Servers " 
description: >-
         A blog on how attackers can starve a network of IP addresses and set up rogue DHCP servers to intercept traffic.
date: 2026-05-10 00:00:00 +0530
categories: [GUIDE, CYBERSECURITY]
tags: [ CEH, Security, Network Security, DHCP]
pin: False
--- 

When you connect to a corporate network, your device shouts into the void for an IP address. The DHCP server hears this and hands one out from its limited pool.

But what if an attacker asks for *all* of them at once?

## Step 1: The Starvation Attack

Standard DHCP operates on absolute trust with zero authentication. An attacker can exploit this using **DHCP Starvation**.

Using network tools like **Yersinia**, the attacker floods the network with thousands of fake `DHCP DISCOVER` packets. Every packet uses a randomly spoofed MAC address. The helpful DHCP server assigns an IP to every fake request, instantly draining its entire pool.

## Step 2: The Rogue Server (The Trap)

The network is now paralyzed. When a legitimate employee connects, the real DHCP server ignores them because its pool is empty.

Enter the trap: The attacker spins up a **Rogue DHCP Server**. When the employee asks for an IP, the attacker's server responds. It happily hands over an IP address, but with a malicious twist,it sets the network's **Default Gateway** to the attacker's IP.

## The Execution: Man-in-the-Middle (MitM)

The Default Gateway is the exit door to the internet. Because the victim accepted the rogue DHCP offer, their computer now believes the attacker's laptop *is* the router.

All of the victim's traffic flows directly into the attacker's machine. The attacker uses tools like Wireshark or Bettercap to capture passwords and strip SSL/TLS encryption before quietly forwarding the traffic to the real internet. The victim never notices.

## Defense: Fixing the Plumbing

Because this targets Layer 2 (Data Link), standard firewalls are useless. Defenses must happen at the switch:

1. **DHCP Snooping:** The switch is configured to only allow DHCP `OFFER` packets from the specific physical port where the authorized server is plugged in.
2. **Port Security (MAC Limiting):** Restricts how many MAC addresses can connect to a single port. If a port generates 3,000 MAC addresses in seconds (the starvation flood), the switch instantly shuts it down.

## Summary for the Exam

* **DHCP Starvation:** A DoS attack draining the DHCP server's IP pool using spoofed MAC addresses.
* **Goal:** To paralyze the real server and pave the way for a Rogue DHCP server.
* **Rogue DHCP Server:** Sets the attacker as the Default Gateway to intercept traffic.
* **Result:** A full Man-in-the-Middle (MitM) compromise.
* **Mitigation:** DHCP Snooping and Switch Port Security.

 