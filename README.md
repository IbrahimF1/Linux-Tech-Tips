# Linux Tech Tips

## Limiting Memory
**Note**
- Modify `MemoryMax` to limit amount of memory
- Change `brave-browser` to your chosen app

### Single Session
```bash
systemd-run --user --scope -p MemoryMax=4G brave-browser
```

### Permanent
```bash
systemd-run --user --scope -p MemoryMax=4G brave-browser %U
```

## Enabling zswap

1) Check if swap exists:
```bash
free -h
```
2) Check if kernel has zswap (Should see `CONFIG_ZSWAP=y`):
```bash
cat /boot/config-$(uname -r) | grep -i zswap
```
3) Edit zswap GRUB config `sudo nano /etc/default/grub`:
```bash
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash zswap.enabled=1 zswap.compressor=lz4 zswap.max_pool_percent=50 zswap.zpool=zsmalloc
```
4) Apply changes:
```bash
sudo update-grub
``` 
5) Add compressor to boot
```bash
sudo su
```
```bash
echo lz4 >> /etc/initramfs-tools/modules
```
```bash
echo lz4_compress >> /etc/initramfs-tools/modules
```
```bash
update-initramfs -u
```
```bash
exit
```

6) Reboot

7) Check if zswap is enabled:
```bash
grep -r . /sys/module/zswap/parameters
```

## Fixing Crackling Audio

1) Navigate to `/usr/share/pipewire/pipewire-pulse.conf`
2) Find `pulse.min.quantum      = 128/48000     # 2.7ms`
3) Change `128/48000` to `256/48000` or `512/48000`
4) Restart PipeWire
```bash
systemctl --user restart wireplumber pipewire pipewire-pulse
```

## Adding Blue Light Filter

1) Install Redshift

2) Create `~/.config/redshift.conf`
```
[redshift]
temp-day=5700
temp-night=3500
location-provider=manual
adjustment-method=randr

[manual]
lat=40.71
lon=-74.01
```

3) Add command to Startup Applications:
```
redshift &
```

## GameMode

1) Install dependencies
```
apt update && apt install meson libsystemd-dev pkg-config ninja-build git dbus-user-session libdbus-1-dev libinih-dev build-essential
```

2) Install GameMode
```
git clone https://github.com/FeralInteractive/gamemode.git
```
```
cd gamemode
```
```
git checkout 1.8.2 # omit to build the master branch
```
```
./bootstrap.sh
```

3) Test GameMode
```
gamemoded -t
```

4) Add to game launch options in Steam
```
gamemoderun %command%
```
