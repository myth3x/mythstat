# MythStat

Lightweight, zero-dependency server fetch and monitoring tool. One bash file.

```
curl -fsSL https://raw.githubusercontent.com/myth3x/mythstat/main/mythstat -o mythstat
sudo install -m 755 mythstat /usr/local/bin/mythstat
```

Or `./mythstat --install`. Run `mythstat`.

## Modes

| Flag | What it does |
|---|---|
| *(none)* | Server specs: OS, kernel, CPU, GPU, RAM, disk, IP, guests, services |
| `-j, --json` | Machine-readable output |
| `-w, --watch [s]` | Live dashboard: CPU, RAM, load, net speed, top processes |
| `-d, --disks` | All filesystems with usage bars |
| `-n, --net` | Interfaces, IPs, traffic and live speed |
| `-t, --top` | Top processes by CPU and memory |
| `-s, --services` | Failed systemd units and key services |
| `-p, --ports` | Listening ports and services |
| `-b, --bench` | Mini benchmark (disk, CPU, ping) |
| `-c, --check` | Threshold check, exit 1 on warning (cron friendly) |
| `-m, --motd` / `--unmotd` | Show on SSH login / disable |
| `-u, --update` | Update, verified with SHA256 |

`--no-color` and `--offline` work with every mode.

## Privacy

MythStat sends **nothing** by default. Network is used only when you ask:
`--update`, `--bench` (ping), `--net` (public IP lookup) and your own
`MYTHSTAT_WEBHOOK`. Use `--offline` to forbid even those.

## Config

`~/.config/mythstat/config` (plain shell):

```sh
MYTHSTAT_RAM_WARN=90
MYTHSTAT_DISK_WARN=90
MYTHSTAT_SWAP_WARN=80
MYTHSTAT_TEMP_WARN=85
MYTHSTAT_LOAD_WARN=4
MYTHSTAT_WEBHOOK="https://discord.com/api/webhooks/..."   # alerts from --check
```

Cron example: `*/5 * * * * mythstat --check --no-color`

## Plugins

`*.sh` files in `/etc/mythstat.d/` or `~/.config/mythstat/plugins/` are loaded
if owned by root or you and not group/world-writable. A plugin can define
`mythstat_cmd_<name>()` (adds `--<name>`), `mythstat_plugin_help()` and
`mythstat_hook_end()`.

## Updates

`mythstat --update` downloads the script and `mythstat.sha256` from
`https://myth3x.com/mythstat` and refuses to install on a hash mismatch or a
version that is not newer. Override with `MYTHSTAT_UPDATE_URL`.

## License

MIT
