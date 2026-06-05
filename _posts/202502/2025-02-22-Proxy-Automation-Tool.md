---
title: Using PConT to Stay Anonymous
description: >-
 Detailed blog on how to use the proxy configuration tool to stay anonymous 
author: anorak
date: 2025-02-22 04:30:00 +0530
categories: [GUIDE,CYBERSECURITY]
tags: [Cybersecurity, Privacy, Linux]
pin: True
---

# PConT - Proxy Automation Tool

## Introduction

Hello Everyone!

You might have heard about how `proxychains` work, but a major drawback is the need to manually search for free proxies and configure the `/etc/proxychains.conf` file.

To solve this, **PConT** automates the entire process. Below, you'll find steps on how to use this tool and the expected output. Thanks for using the tool, and feedback is appreciated!


## What is PConT?

**PConT** (Proxy Connection Tool) is a powerful and flexible proxy automation tool designed to streamline proxy acquisition and management. Built with Python, it caters to cybersecurity professionals and enthusiasts who need an efficient way to handle proxy connections. (**Saves a lot of time!**)

### Features

- **Automated Proxy Acquisition**: Automatically scrape and retrieve proxies from the [proxyscrape API](https://www.proxyscrape.com/).
- **Integration Ready**: Easily integrate proxy management into your existing tools.
- **Configurable Execution**: Customize script behavior as per your requirements.
- **Proxy Checker**: Automatically checks if the proxy is active (aka Downdetector).
- **Auto-Configuration**: No need to manually edit lengthy `.conf` files—get your network setup ready in seconds.


## Installation Guide

### Step 1: Clone the Repository
```bash
git clone https://github.com/neithium/PConT.git
```

### Step 2: Make Scripts Executable
```bash
chmod +x proxychains-setup.sh
chmod +x proxychains-reset.sh
chmod +x proxychains-help.sh
```

### Step 3: Install Proxychains (If Not Installed)
```bash
sudo apt install proxychains
```

### Step 4: Run the Setup Script
```bash
./proxychains-setup.sh
```

### Step 5: Need Help? Refer to Docs or Run:
```bash
./proxychains-help.sh
```

### Step 6: Reset Configuration (If Required)
```bash
./proxychains-reset.sh
```

## Demo

### Screenshot:
![img](/assets/img/202502/Screenshot.png){: width="500" height="500"  .center}

### Video:
{% include embed/youtube.html id='IwWsFYwWFXs' %}



## Important Notes

- The proxies are sourced from [proxyscrape.com](https://www.proxyscrape.com/). I do not own them.
- A new feature is coming soon to mitigate potential proxy-related issues.
- While I cannot guarantee 100% safety, these proxies are **safer than most VPNs**.


### Feedback

Your feedback is valuable! If you encounter issues or have suggestions, feel free to contribute or raise an issue on the [GitHub repo](https://github.com/neithium/PConT).

Happy proxy chaining! 🚀
