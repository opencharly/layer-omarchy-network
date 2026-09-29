# omarchy-network

Omarchy's networking, bluetooth, printing and firewall stack as a charly layer —
**machine-only**.

The `omarchy-network` candy installs the daemons that own a machine's network,
bluetooth, print and firewall state: NetworkManager, bluez, CUPS and avahi, plus
`ufw` and Omarchy's `ufw-docker`. None of it belongs in a pod — NetworkManager,
bluez, cups and avahi each claim host devices or the system D-Bus, and `ufw`
needs `NET_ADMIN` and the host's netfilter tables. This candy is composed by the
**machine images only**; no unit is enabled here, because enabling them is the
machine image's job.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `omarchy-network` |
| Requires | `layer-omarchy-base` (the foundation layer) |
| Network | `networkmanager`, `nss-mdns`, `avahi`, `wireless-regdb`, `bolt` |
| Bluetooth | `bluez`, `bluez-tools`, `bluez-utils` |
| Printing | `cups`, `cups-browsed`, `cups-filters`, `cups-pdf`, `system-config-printer` |
| Firewall | `ufw`, `ufw-docker` |
| Service / port | none enabled (machine images enable the units) |

## How to use it

Compose the layer by pinning the member candy's sub-path in a **machine**
image's `candy:` list (not a pod):

```yaml
my-omarchy-machine:
  candy:
    base: omarchy
    candy:
      - '@github.com/opencharly/layer-omarchy-base/candy/omarchy-base:v2026.242.0701'
      - '@github.com/opencharly/layer-omarchy-network/candy/omarchy-network:v2026.242.0835'
```

## Layout

- `charly.yml` — repo shape: the `discover:` rule that finds the member candy.
- `candy/omarchy-network/charly.yml` — the candy entity (the `distro:` package
  arm and the `plan:` `check:` assertions).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Foundation: `/charly-distros:omarchy-base`.
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella.
