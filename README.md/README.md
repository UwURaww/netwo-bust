# README

## NETWO BURST

```
▄██████████████▄▐█▄▄▄▄█▌
██████▌▄▌▄▐▐▌███▌▀▀██▀▀
████▄█▌▄▌▄▐▐▌▀███▄▄█▌
▄▄▄▄▄██████████████▀
```

Terminal-first network load testing with live monitoring. Designed for **controlled environments**.

[![License](https://img.shields.io/badge/license-MIT-red.svg)](../LICENSE/) ![Platform](https://img.shields.io/badge/platform-Termux-blue.svg) ![Bash](https://img.shields.io/badge/bash-5.0+-green.svg)

**Jump to:** [Install](./#install-termux--arch) · [Quick start](./#quick-start-lab-only) · [Dashboard](./#reading-the-dashboard) · [Configuration](./#configuration) · [Troubleshooting](./#troubleshooting) · [Legal](./#legal--acceptable-use)

{% hint style="warning" %}
Use only on systems you own or have explicit written permission to test. Do not run this against public services or third-party networks.
{% endhint %}

### Overview

NETWO BURST runs a repeatable network load scenario. It also shows live system and link signals.

Use it to validate stability. Use it to spot bottlenecks. Keep tests inside a lab.

### Key features

* **Multi-process workers**
  * Run parallel workers (“threads”).
  * Track worker status in real time.
* **Live monitoring**
  * Live upload/download estimates.
  * Basic system health signals.
* **Termux-friendly**
  * Built for Termux + proot-distro (Arch).
  * Lightweight dependencies.

<details>

<summary>What this is not</summary>

This is not a tool for disrupting networks. Do not use it as a denial-of-service utility. If you need public-facing load testing, use a managed service.

</details>

### Requirements

* Android device with **Termux**
* `proot-distro` with an **Arch Linux** install
* Packages: `bash`, `curl`, `iputils`, `bc`, `git`

### Install (Termux + Arch)

{% stepper %}
{% step %}
### Install and enter Arch

```bash
pkg update && pkg upgrade -y
pkg install proot-distro -y

proot-distro install archlinux
proot-distro login archlinux
```

{% hint style="info" %}
Install Termux from F-Droid or GitHub releases. Avoid random Play Store builds.
{% endhint %}
{% endstep %}

{% step %}
### Install dependencies

```bash
pacman -Syu --noconfirm
pacman -S --noconfirm git curl iputils bc
```
{% endstep %}

{% step %}
### Clone and run

```bash
git clone https://github.com/UwURaww/netwo-bust.git
cd netwo-bust

chmod +x netwo_burst.sh
./netwo_burst.sh

or
sudo bash netwo_burst.sh
```
{% endstep %}
{% endstepper %}

<details>

<summary>One-liner install (same steps, single command)</summary>

```bash
proot-distro login archlinux -- bash -c "pacman -Syu --noconfirm && pacman -S --noconfirm git curl iputils bc && git clone https://github.com/UwURaww/netwo-bust.git && cd netwo-bust && chmod +x netwo_burst.sh && ./netwo_burst.sh"
```

</details>

### Quick start (lab-only)

1. Pick a **test target you own**.
2. Run the script.
3. Start with a small worker count.
4. Increase slowly while watching health signals.

```bash
./netwo_burst.sh
```

Stop anytime with `Ctrl+C`.

{% hint style="info" %}
For clean results, test in a sandbox. Use a test VLAN, isolated Wi‑Fi, or a local VM.
{% endhint %}

### Reading the dashboard

The UI is meant to be scanned fast. Names can vary by version.

* **Target**
  * The system under test.
* **Workers / threads**
  * Parallel processes generating load.
  * More is not always better.
* **Uptime**
  * How long the current run has been active.
* **Upload / download**
  * A rough estimate of link throughput.
  * Use it for trends, not billing-grade accuracy.
* **Per-worker stats**
  * Helps you spot dead workers.
  * Helps you spot skewed distribution.
* **System health**
  * CPU load and thermal throttling can skew results.
  * Watch for drops in throughput and rising latency.

<details>

<summary>How to get useful numbers</summary>

* Disable background downloads.
* Keep the device plugged in.
* Keep the screen on.
* Use the same access point and distance.
* Run at least two passes for comparison.

</details>

### How it works (high-level)

NETWO BURST drives a network test using standard userland tools. It runs multiple worker processes in parallel.

It samples local interface counters periodically. It renders a live view of trends during the run.

{% hint style="info" %}
This section stays intentionally high-level. It’s meant for understanding, not misuse.
{% endhint %}

### Limitations

* **Throughput numbers are approximate**
  * Good for trending and comparisons.
  * Not a substitute for lab-grade measurement gear.
* **Phone thermals matter**
  * Throttling can look like “network issues”.
* **Wi‑Fi noise is real**
  * Channel congestion can dominate results.
  * Re-test at different times.

### Test methodology (recommended)

1. **Define scope**
   * What is the target.
   * What is allowed traffic.
   * What is the max duration.
2. **Baseline**
   * Run with minimal load.
   * Capture normal throughput and latency.
3. **Ramp**
   * Increase workers gradually.
   * Stop when you hit instability.
4. **Record**
   * Save terminal output.
   * Note device model, network type, and target details.
5. **Stop condition**
   * Temperature threshold.
   * Packet loss threshold.
   * Router or service health degradation.

### FAQ

**Can I run this against a random public IP?**

No. Only test systems you own or have written permission to test.

**Why does performance drop over time?**

Most often: thermal throttling or Wi‑Fi congestion. Use a shorter timebox and compare multiple runs.

**Why do workers show uneven stats?**

The OS scheduler and network stack can skew distribution. Look for dead workers or a saturated link.

### Troubleshooting

* **Permission denied**
  * `chmod +x netwo_burst.sh`
* **Missing commands**
  * `pacman -S --noconfirm bc curl iputils`
* **Wrong network interface**
  * Set it before running:
    * `export NET_IFACE="wlan0"`
* **No traffic / no readings**
  * Confirm you have network access inside Arch.
  * Verify DNS and routing.
  * Try again with a known reachable lab host.

### Configuration

* Use a custom interface:
  * `export NET_IFACE="wlan0"`

<details>

<summary>Safety notes</summary>

* Prefer conservative settings.
* Avoid testing across networks you don’t fully control.
* Always timebox runs.
* Always define a stop condition before starting.

</details>

### Legal + acceptable use

You are responsible for how you run this. Get written permission. Keep scope tight.

**Allowed**

* Your own networks and hosts.
* Explicit, written authorization from the owner.
* Controlled labs and academic environments.

**Not allowed**

* Public targets you don’t own.
* Third-party services.
* Any attempt to degrade availability.

{% hint style="warning" %}
Unauthorized testing can be illegal. It can also harm others. Do not run outside an approved scope.
{% endhint %}

### Contributing

* Fork the repo.
* Create a branch.
* Open a PR with a clear description and test notes.

### License

MIT. See the repository license file for details.

### Support

* Issues: GitHub Issues (preferred)
