---
type: note
tags: [os, linux, sysadmin]
---

Resources to learn linux: overthewire.org, hackthebox linux, Redhat certified system administrator - EX200

A working model of Linux for administering, troubleshooting, and securing it. The unifying idea: **almost everything is a file or a process** — config is text under `/etc`, live state is exposed as files (`/proc`, `/sys`), and work is done by processes you can list, signal, and trace. Once that clicks, the system stops being a black box.

## 1. Architecture

```
User apps → Shell (bash) → System libs (glibc) → [ syscall interface ] →
KERNEL: scheduler · memory manager · VFS · network stack · drivers → Hardware
```

- **Kernel mode (Ring 0)** = unrestricted hardware access; a crash here = kernel panic. **User mode (Ring 3)** is isolated — apps reach the kernel only through **system calls** (`open/read/write/fork/exec/socket`…), the single controlled crossing. **glibc** wraps raw syscalls in C functions; trace them with `strace`.
- **Kernel subsystems:** **CFS scheduler** (what runs, context switching), **memory manager** (per-process virtual address spaces, paging, swap, protection), **VFS** (uniform `open/read/write` over ext4/XFS/Btrfs/NFS/proc — the "everything is a file" layer), **network stack** (sockets→TCP/UDP→IP→driver, Netfilter, namespaces), **drivers** (loadable modules: `lsmod/modprobe`).
- **Process tree:** every process but PID 1 (`systemd`) has a parent; orphans re-parent to PID 1; **zombies (Z)** are dead children the parent hasn't reaped.

## 2. Boot sequence & recovery

```
UEFI/BIOS (POST) → GRUB2 (loads vmlinuz + initramfs) → kernel (mounts initramfs, loads disk drivers, mounts real /) → PID 1 systemd → target → services → login
```
Each stage fails distinctly: **GRUB** ("grub rescue>") → reinstall from live media; **kernel panic** → boot previous kernel from the GRUB menu; **initramfs** ("can't find root") → rebuild (`update-initramfs -u` / `dracut -f`); **systemd** drops to an **emergency shell** → almost always a bad `/etc/fstab`.

**systemd targets** replace runlevels: `rescue` (1, single-user), `multi-user` (3, server default), `graphical` (5), `emergency` (most minimal). `systemctl get-default` / `set-default`, `systemctl isolate rescue.target`.

Essential recovery moves:
```bash
journalctl -b -1 -p err              # why did it die before the last reboot?
systemd-analyze blame                # slowest units (long boot)
mount -o remount,rw / ; nano /etc/fstab ; mount -a   # fix emergency-mode fstab, then test
# Reset a lost root password: at GRUB press 'e', append init=/bin/bash, then:
mount -o remount,rw / ; passwd root ; exec /sbin/init
```
> Test any fstab change with `mount -a` and `findmnt --verify` **before** rebooting; add `nofail` to non-critical entries so a missing disk can't block boot.

## 3. Filesystem hierarchy

`/etc` config (most security-critical) · `/var/log` logs · `/home`,`/root` homes · `/proc`,`/sys` virtual (live kernel state) · `/dev` devices · `/tmp`,`/dev/shm` world-writable (attacker staging) · `/usr` programs · `/boot` kernel.

| Path | Why it matters |
|---|---|
| `/etc/passwd` / `/etc/shadow` | Users (world-readable) / password hashes (root-only) |
| `/etc/sudoers` | Who escalates to root — privesc if misconfigured (edit with `visudo`) |
| `/var/log/auth.log` | SSH/sudo/su — primary forensic source |
| `~/.ssh/authorized_keys` | SSH key access — persistence target |

## 4. Permissions & ownership

`-rw-r--r--` = type + owner/group/other (`r=4 w=2 x=1` → `rwx=7`, `rw-=6`, `r-x=5`). `chmod 640 file`, `chown user:group file` (`-R` recursive).

**Special bits:** **SUID** (`chmod u+s`) runs as the file's owner — `find / -perm -4000 -type f 2>/dev/null` is the key privesc audit. **SGID** (group/dir inheritance), **sticky** (`chmod +t /tmp` — only owner deletes). **Capabilities** (`getcap -r /`) split root's power; `cap_setuid`, `cap_sys_ptrace`, `cap_dac_override` are dangerous.

## 5. Users & groups

`/etc/passwd` (`name:x:UID:GID:comment:home:shell`; UID 0 = root, ≥1000 = humans, `nologin` = no login) + root-only `/etc/shadow`. Commands: `id`, `useradd -m -s /bin/bash`, `passwd`, `usermod -aG sudo alice`, `userdel -r`, `visudo`, `sudo -l`. Audit `awk -F: '($3==0){print $1}' /etc/passwd` for backdoor root accounts.

## 6. Processes

```bash
ps aux --sort=-%cpu | head ; pgrep -u alice ; pstree -p
kill -15 PID  # SIGTERM graceful; -9 SIGKILL force; -HUP reload
cat /proc/PID/{cmdline,environ,status,maps}    # live process data
```
**STAT states:** **R** running, **S** normal sleep, **D** uninterruptible I/O wait (can't even be killed → investigate storage), **Z** zombie (fix the parent), **T** stopped.

## 7. Services — systemd

```bash
systemctl status|start|stop|restart|reload nginx
systemctl enable --now nginx          # start now + on boot
systemctl list-units --type=service --state=failed
journalctl -u nginx -f                 # follow a service's logs
systemctl daemon-reload                # after editing unit files
```
A unit (`/etc/systemd/system/app.service`) declares `ExecStart`, `User`, `Restart=on-failure`, `WantedBy=multi-user.target`. **systemd timers** (`OnCalendar=`, `Persistent=true` for catch-up) are cron's more powerful sibling.

## 8. Packages

| Family | Update | Install | Owns a file |
|---|---|---|---|
| Debian/Ubuntu (APT) | `apt update && apt upgrade` | `apt install x` | `dpkg -S /path` |
| RHEL/Fedora (DNF) | `dnf update` | `dnf install x` | `rpm -qf /path` |
| Arch (pacman) | `pacman -Syu` | `pacman -S x` | `pacman -Qo /path` |

## 9. Networking

```bash
ip a ; ip r ; ss -tlnp           # addresses ; routes ; listening ports + process
dig example.com ; ping -c4 8.8.8.8 ; mtr 8.8.8.8 ; tcpdump -i any -nn port 80
```
**Connectivity ladder:** `ip a` (have IP?) → `ping 127.0.0.1` (stack?) → ping gateway (LAN?) → ping `8.8.8.8` (internet?) → `dig` (DNS?) → `ss -tlnp` (service up?) → firewall (`nft list ruleset`). The first step that fails localizes the problem.

## 10. Storage

```
disk (/dev/sda) → partition → [LVM: PV→VG→LV] → filesystem (ext4/xfs) → mount point
```
```bash
lsblk -f ; blkid ; df -h ; df -i      # df -i: out of INODES is also "disk full"
mkfs.ext4 /dev/sdb1 ; mount /dev/sdb1 /data
# Persistent: add UUID=… line to /etc/fstab (prefer UUID over /dev/sdX)
lvextend -r -L +20G /dev/vg0/data     # grow LV + filesystem online (LVM's killer feature)
smartctl -a /dev/sda                  # drive health
```
Two ways a disk is "full": no bytes (`df -h`, find big with `du -xh / | sort -rh`) or no inodes (`df -i`, millions of tiny files). Watch for deleted-but-open files holding space: `lsof +L1` → restart that process. **xfs can grow but never shrink.**

## 11. Performance — find the bottleneck (USE method)

For each resource check **Utilization, Saturation, Errors**. 60-second triage:
```bash
uptime ; vmstat 1 5 ; free -h ; mpstat -P ALL 1 ; iostat -xz 1 ; ss -s
```
- **Load average** = runnable **plus D-state (I/O-blocked)** processes vs `nproc`. High load + low CPU = an **I/O** problem (confirm `%wa`).
- **CPU:** high `%usr` = app code; high `%sys` = kernel/syscalls; high `%iowait` = really disk.
- **Memory:** read `free -h` **available** (not "free") — buff/cache is reclaimable; sustained `si/so` swap = thrashing; OOM kills show in `dmesg`.
- **Disk:** `iostat -xz` `%util`≈100 + high `await` = saturated; find the PID with `iotop`.
- **`sar`** (sysstat) records history so you can answer "what did 3am look like?"

## 12. Troubleshooting playbooks (symptom → fix)

All are instances of: define symptom → check resources (`df -h; free -h; uptime; systemctl --failed`) → read logs (`journalctl -p err -b`) → isolate the layer → test one hypothesis → fix & verify.

- **Slow / high load** → `top`, `vmstat 1` (`wa`=disk, `si/so`=swap); renice or kill the runaway.
- **Disk full** → `df -h`/`df -i`, `du -xh`, `journalctl --vacuum-size=200M`, `lsof +L1`.
- **OOM** → `dmesg | grep -i oom`; add RAM/swap, cap the service (`MemoryMax=`), fix the leak.
- **Service won't start** → `systemctl status` + `journalctl -u x`; common: config error, port in use (`ss -tulpn`), wrong `User=`, forgot `daemon-reload`.
- **No network** → the connectivity ladder (§9).
- **Permission denied** → `ls -la`, `namei -l /path` (a parent dir missing `x`?), `getfacl`, SELinux denial (`ausearch -m AVC`).
- **Can't SSH** → `ssh -v`; server side `systemctl status sshd`, `ss -tlnp | grep :22`, `sshd -t`, fail2ban ban.
- **Compromise triage** → `w; last; lastb`, `ps auxf`, `ss -tulpn`, check cron/authorized_keys/UID-0/SUID, `find /etc -mtime -7`.

## 13. Hardening checklist

| Control | How |
|---|---|
| Audit UID-0 accounts | `awk -F: '($3==0){print $1}' /etc/passwd` (only `root`) |
| Disable root SSH + passwords | `PermitRootLogin no`, `PasswordAuthentication no` → keys only |
| Audit SUID/SGID | `find / -perm /6000 -type f` against a baseline |
| Kernel `sysctl` | `randomize_va_space=2`, `kptr_restrict=2`, `tcp_syncookies=1`, `rp_filter=1`, `yama.ptrace_scope=1` |
| MAC enforcing | **AppArmor** (`aa-enforce`) / **SELinux** (`SELINUX=enforcing`) |
| Audit logging | `auditd` watching passwd/shadow/sudoers/sshd_config |
| Reduce surface | purge telnet/FTP/rsh/NIS, X11 & compilers on servers |
| Brute-force / patching | `fail2ban` (maxretry 3), `unattended-upgrades` |
| Mounts & boot | `noexec,nosuid,nodev` on `/tmp`; GRUB password; run `lynis audit system` |

SSH specifics: `MaxAuthTries 3`, modern ciphers (`chacha20-poly1305`, `curve25519-sha256`), `AllowUsers`, idle timeout; validate with `sshd -t`.

## 14. Distributions

| Distro | Pkg | Niche |
|---|---|---|
| Ubuntu / Debian | apt | Servers, cloud, desktop |
| RHEL / Alma / Rocky / Fedora | dnf | Enterprise / upstream |
| Arch / Manjaro | pacman | Rolling, DIY |
| Kali / Parrot | apt | Penetration testing |
| Alpine | apk | Minimal — containers, IoT |

Related: [[Windows]] · [[04 Operating Systems|OS concepts]] · [[System Hardening]] · [[10 DevOps|containers]] · [[05 Networking]] · [[06 Cybersecurity|Ethical Hacking]]
