# NixOS-Infect (Fork by fnltochka)

A script to install **NixOS** on non-NixOS hosts.  
This is a **fork** of [elitak/nixos-infect](https://github.com/elitak/nixos-infect) with some minimal changes:

1. **Disabled firewall** by default in the generated NixOS configuration.  
2. **Generates a random root password** (shown before reboot).  
3. Checks for the presence of both `/etc/nixos/configuration.nix` and `/etc/nixos/hardware-configuration.nix` before skipping generation.  

Otherwise, the script still follows the same approach as the original.  

---

## What is this?
NixOS-Infect is so named because of the high likelihood of rendering a system inoperable.  
Use with extreme caution and preferably only on newly provisioned systems.

This forked script has successfully been tested on multiple hosting providers and plans:

- [DigitalOcean](https://www.digitalocean.com/products/droplets/)
- [Hetzner Cloud](https://www.hetzner.com/cloud)
- [Vultr](https://www.vultr.com/)
- [Interserver VPS](https://www.interserver.net/vps/)
- [Tencent Cloud Lighthouse](https://cloud.tencent.com/product/lighthouse)
- [OVHcloud](https://www.ovh.com/)
- [Oracle Cloud Infrastructure](https://www.oracle.com/cloud/)
- [GalaxyGate](https://galaxygate.net)
- [Cockbox](https://cockbox.org)
- [Google Cloud Platform](https://cloud.google.com/)
- [Contabo](https://contabo.com)
- [Liga Hosting](https://ligahosting.ro)
- [AWS Lightsail](https://aws.amazon.com/lightsail/)
- [Windcloud](https://windcloud.de/)
- [Clouding.io](https://clouding.io)
- [Scaleway](https://scaleway.com)
- [RackNerd](https://my.racknerd.com/index.php?rp=/store/black-friday-2022)

Should you find that it works on another hosting provider, feel free to open a pull request updating this README.

---

## Motivation

The original motivation for `nixos-infect` is that existing solutions—like `nixos-assimilate` or `nixos-in-place`—were either incomplete, overly complex, or broken for certain environments. This script (and this fork) aim to preserve a straightforward approach to replacing the underlying OS with NixOS.

---

## Usage

1. **Read and understand the [script](./nixos-infect)**, as it **destroys** your existing root filesystem.
2. Provision a fresh VPS or dedicated server running a non-Nix distro (Debian, Ubuntu, CentOS, etc.).
3. Ensure root has SSH key access (no password or minimal password). This is critical because once NixOS is installed, root login over SSH will require an SSH key (unless you use the randomly generated password).
4. Run something like:
   ```bash
   curl https://raw.githubusercontent.com/fnltochka/nixos-infect/master/nixos-infect \
     | NIX_CHANNEL=nixos-23.05 bash -x
   ```
   You can specify a different `NIX_CHANNEL`, e.g. `nixos-24.11`.

At the end of the process, your system reboots into NixOS.  
The script prints out the randomly generated **root password** before reboot, if none was previously set.  
**Remember** to store it safely or change it immediately.

---

## Hosting Provider Notes

Below are the same (or very similar) instructions and test logs as the original repository. We have retained them for convenience. In practice, usage on any provider should be similar: spin up a fresh instance (Ubuntu, Debian, etc.), then run the script.

<details>
<summary>DigitalOcean</summary>

You can use [DigitalOcean user data](https://docs.digitalocean.com/products/droplets/how-to/insert-commands-when-creating-droplets-with-user-data/) to run `nixos-infect` automatically. Example:

```yaml
#cloud-config

runcmd:
  - curl https://raw.githubusercontent.com/fnltochka/nixos-infect/master/nixos-infect | PROVIDER=digitalocean NIX_CHANNEL=nixos-23.05 bash 2>&1 | tee /tmp/infect.log
```

<details>
<summary>Click for more tested distributions</summary>

|Distribution|       Name      | Status    | test date|
|------------|-----------------|-----------|----------|
|Ubuntu      |20.04 x64        |**success**|2022-03-23|
|Ubuntu      |22.04 x64        |**success**|2023-06-05|
|Ubuntu      |22.10 x64        | _failure_ |2023-06-05|
|Ubuntu      |23.10 x64        | _failure_ |2023-11-16|
|Debian      |10.3 x64         |**success**|2020-03-30|
|Debian      |11   x64         |**success**|2023-11-12|
</details>
</details>

<details>
<summary>Hetzner Cloud</summary>

```yaml
#cloud-config
runcmd:
  - curl https://raw.githubusercontent.com/fnltochka/nixos-infect/master/nixos-infect | PROVIDER=hetznercloud NIX_CHANNEL=nixos-23.05 bash 2>&1 | tee /tmp/infect.log
```

<details>
<summary>Click for tested distributions</summary>

|Distribution|Name         | Status    | test date|
|------------|-------------|-----------|----------|
|Debian      |12 aarch64   |**success**|2023-09-02|
|Ubuntu      |22.04 x64    |**success**|2023-04-29|
</details>
</details>

<details>
<summary>... and so on for other providers</summary>

*(Same as the original README content, with tested distributions, etc.)*

</details>

---

## Disclaimer

Use at your own risk!  
This script removes and replaces your OS with NixOS. Make sure you have backups or that your server is easily re-deployable.

If you encounter issues, please open an issue or pull request in this fork's repository:  
[https://github.com/fnltochka/nixos-infect](https://github.com/fnltochka/nixos-infect).

---

**Thanks to the original authors of [`nixos-infect`](https://github.com/elitak/nixos-infect)** and all contributors.  
This fork aims to provide just a few tweaks while preserving compatibility with the original approach.
