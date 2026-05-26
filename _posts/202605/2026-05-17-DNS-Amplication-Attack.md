---    
title:  " DNS Amplification Attack " 
description: >-
         A blog on how attackers can use DNS servers to amplify their DDoS attacks.
date: 2026-05-17 00:00:00 +0530
categories: [GUIDE, CYBERSECURITY]
tags: [ CEH, Security, Network Security, DNS]
pin: False
--- 

## The Prank Call Analogy

Imagine you want to harass a rival business. You *could* just go to their storefront and yell at them, but they’d immediately see your face and call the cops.

Instead, you call every pizza restaurant in the city. You order 100 large pizzas from each, and when they ask for the delivery address, you give them the address of your rival.

An hour later, dozens of delivery trucks show up at your rival's business at the same time, completely blocking their doors and shutting down their operations. You used a small amount of effort (a phone call) to deliver a massive payload (the pizzas) using a third party.

This is exactly how a **DNS Amplification Attack** works.

![DNS Amplification Attack](/assets/img/202605/DNS.png){: .center}

## The Flaw: Why the Internet Believes Liars

To pull off the pizza prank, the restaurant has to blindly believe that *you* are the person at the delivery address.

In networking, this flaw exists in the **UDP Protocol**. Unlike TCP, which uses a strict 3-way handshake to verify who it's talking to, UDP is connectionless. It’s fire-and-forget.

Because DNS (the internet's phonebook) runs on UDP, an attacker can easily **spoof their IP address**. They craft a packet asking a question, but write the victim's IP address on the return envelope.

## The Attack: Small Question, Massive Answer

**Step 1: Finding the Amplifiers**
The attacker scans the internet for "Open DNS Resolvers" misconfigured DNS servers that will answer queries from absolutely anyone.

**Step 2: The Spoofed Query**
The attacker sends a DNS request to the Open Resolver. But they don't ask for a simple IP address. They ask for the `ANY` record of a massive domain (like a government site or a major tech company), which contains cryptographic keys, mail routing data, and text records.

And, crucially, they spoof the source IP so it looks like the *victim's server* is the one asking the question.

**Step 3: The Amplification**
The attacker’s query is tiny maybe **60 bytes** of data.
But the Open Resolver's answer containing the `ANY` record? That could be **3,000+ bytes**.

This is an **amplification factor of 50x**.

**Step 4: The Obliteration**
The attacker uses a small botnet to send thousands of these 60-byte requests to hundreds of Open Resolvers every second.
The Resolvers do their job. They simultaneously blast the victim's server with gigabytes of unrequested, 3000-byte DNS responses.

The victim's network pipes instantly clog, and their website drops offline.

## Defense: Stopping the Trucks

Because the traffic hitting the victim consists of entirely legitimate DNS responses, traditional firewalls struggle to block it without breaking real internet traffic.

1. **Ingress Filtering (BCP38):** The ultimate fix. Internet Service Providers (ISPs) must configure their routers to immediately drop outbound packets that have a spoofed IP address. If you can't spoof the address, the attack is impossible.
2. **Close Open Resolvers:** Network admins must configure their DNS servers to only answer queries from their own internal employees, not the entire public internet.
3. **Anycast Networks (Cloudflare/Akamai):** Victims can hide behind massive cloud scrubbing centers. Instead of a single server trying to absorb the flood, the traffic is spread out across thousands of servers worldwide, absorbing the blow.

## Summary for the Exam

* **DNS Amplification:** An asymmetric DDoS attack where a small spoofed request generates a massive response directed at a victim.
* **Core Vulnerability:** The connectionless nature of the **UDP protocol**, which allows trivial IP spoofing.
* **The Mechanism:** Querying Open DNS Resolvers for `ANY` or `TXT` records.
* **Mitigation:** Network Ingress Filtering (BCP38) and properly securing DNS resolvers.

 
 