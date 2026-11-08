# redux-arch-settings

Tweaks and optimizations for Arch-based systems originally based on [CachyOS-Settings](https://github.com/CachyOS/CachyOS-Settings).

These tweaks are made for my setup and may not work as expected for your system, if you really want to use these tweaks please read them carefully in the section below, I recommend installing these settings through my [archie script](https://github.com/ReduxFlakes/archie) since it takes into account the environment and installs needed dependencies.

## Tweaks

### Udev Rules
Udev rules automatically apply system configurations upon device detection or state changes.
* **Audio Power Management**: Manages `snd-hda-intel` power saving to mitigate audio crackling, disabling it when AC-powered and re-enabling on battery.
* **ZRAM Swap Optimization**: When ZRAM initializes, raises `vm.swappiness` to `150` and disables Zswap so anonymous pages are compressed instead of flushed from the page cache.
* **Device Permissions**: Sets `rtc0` and `hpet` device group to "audio" for proper application access.
* **CPU DMA Latency Access**: Sets permissions for the `cpu_dma_latency` device.

### Sysctl
Sysctl parameters modify kernel behavior at runtime for system-wide performance and stability.
* **Memory & I/O Management**: Sets `vm.swappiness=100` at boot, along with `vm.vfs_cache_pressure`, `vm.dirty_bytes`, `vm.dirty_background_bytes`, and `vm.dirty_writeback_centisecs` for balanced memory usage and efficient disk I/O. Disables `vm.page-cluster`.
* **System Stability & Security**: Disables `kernel.nmi_watchdog`, and restricts `kernel.kptr_restrict`.
* **Logging & Network**: Configures `kernel.printk` to hide messages from console, increases `net.core.netdev_max_backlog`, and sets `fs.file-max`.

### Modprobe
Modprobe configurations control module loading and behavior for hardware-specific optimizations.
* **Watchdog Module Blacklist**: Prevents loading of Intel TCO watchdog timer.

### Systemd
Systemd unit and configuration files for streamlined boot, resource management, and service control.
* **Journal Log Limits**: Sets `journald` size limit to 50MB.
* **Service Timeouts**: Defines `DefaultTimeoutStartSec` (15s) and `DefaultTimeoutStopSec` (10s) for system and user services.
* **File Descriptor Limits**: Increases `DefaultLimitNOFILE` for both system (2048:2097152) and user (1024:1048576) services.
* **User Service Resource Delegation**: Delegates CPU, cpuset, IO, memory, and pids to user services.

### Tmpfiles
Configurations for temporary file cleanup and Transparent Huge Page (THP) behavior.
* **Coredump Retention**: Clears coredumps older than 3 days.
* **THP Defragmentation**: Sets `transparent_hugepage/defrag` to `defer+madvise` for tcmalloc-using applications.
* **THP Shrinker**: Configures `khugepaged/max_ptes_none` for Kernel 6.12+ to optimize THP memory usage.

### Network & Modules
* **NetworkManager DNS**: Uses `systemd-resolved` for DNS resolution.
* **NTSync Module**: Loads the `ntsync` kernel module at boot.
* **NetworkManager Wi-Fi Power Save**: Disables Wi-Fi power saving for all connections to reduce latency and packet loss.

### Audio & Debugging
* **Realtime Audio Priority**: Grants the `@audio` group `rtprio 99` via `/etc/security/limits.d`.

## Resources & Credits

- [CachyOS-Settings](https://github.com/CachyOS/CachyOS-Settings)
- [Improving performance - Arch Linux wiki](https://wiki.archlinux.org/title/Improving_performance)
- [Lower WiFi latency - Fedora Discussion](https://discussion.fedoraproject.org/t/tip-lower-wifi-latency-by-disabling-wifi-power-management/)
