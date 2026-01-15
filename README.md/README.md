# README

## NETWO BURST

Terminal-first network load testing + live monitoring for **controlled environments**.

[![License](https://img.shields.io/badge/license-MIT-red.svg)](../LICENSE/) ![Platform](https://img.shields.io/badge/platform-Termux-blue.svg) ![Bash](https://img.shields.io/badge/bash-5.0+-green.svg)

{% hint style="warning" %}
Use only on systems you own or have explicit written permission to test. Do not run this against public services or third-party networks.
{% endhint %}

### What you get

* Multi-process “threads” for repeatable load generation.
* Live dashboard for throughput and per-thread status.
* Lightweight stack. Designed for Termux + proot-distro (Arch).

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
```
{% endstep %}
{% endstepper %}

<details>

<summary>One-liner install (same steps, single command)</summary>

```bash
proot-distro login archlinux -- bash -c "pacman -Syu --noconfirm && pacman -S --noconfirm git curl iputils bc && git clone https://github.com/UwURaww/netwo-bust.git && cd netwo-bust && chmod +x netwo_burst.sh && ./netwo_burst.sh"
```

</details>

### Usage

```bash
./netwo_burst.sh
```

* Follow the prompts.
* Start small. Scale only inside a lab or isolated network.
* Stop anytime with `Ctrl+C`.

{% hint style="info" %}
If you’re doing performance validation, run from a test VLAN or sandbox. Keep logs of authorization and scope.
{% endhint %}

### Troubleshooting

* **Permission denied**
  * `chmod +x netwo_burst.sh`
* **Missing commands**
  * `pacman -S --noconfirm bc curl iputils`
* **Wrong network interface**
  * Set it before running:
    * `export NET_IFACE="wlan0"`

### Configuration

* Use a custom interface:
  * `export NET_IFACE="wlan0"`

<details>

<summary>Notes on safety</summary>

* Prefer rate-limited tests over “maxed out” tests.
* Avoid testing across networks you don’t fully control.
* Always define a stop condition before starting.

</details>

### Contributing

* Fork the repo.
* Create a branch.
* Open a PR with a clear description and test notes.

### License

MIT. See the repository license file for details.

### Support

* Issues: GitHub Issues (preferred)
