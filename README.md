# Linux Tech Tips

## Changing Boot Mode

### Setting Multi-User (Text Only) Mode as Default
```bash
sudo systemctl set-default multi-user.target
```

### Setting Graphical Mode as Default
```bash
sudo systemctl set-default graphical.target
```

### Booting into Graphical Mode from Multi-User Mode
```bash
sudo systemctl isolate graphical.target
```
```bash
sudo systemctl start gdm3
```

## Choose Default Monitor for VSync
1) Identify connected monitors
```bash
xrandr -q
```
2) Set default monitor in environment variables (Replace DP-0 with chosen monitor)
```bash
export __GL_SYNC_DISPLAY_DEVICE=DP-0
```

## Limiting Memory
**Note**
- Modify `MemoryMax` to limit amount of memory
- Change `brave-browser` to your chosen app

### Single Session
```bash
systemd-run --user --scope -p MemoryMax=6G brave-browser
```

### Permanent
```bash
systemd-run --user --scope -p MemoryMax=6G brave-browser %U
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




# Terminal Dev Tips

## [kmscon](https://github.com/kmscon/kmscon) - upgrading Linux virtual console

### Install [libtsm](https://github.com/kmscon/libtsm)
1) Download zip from Releases
2) Extract zip
3) Enter libtsm folder
4) Run the following in the libtsm folder:
```bash
meson setup -Dtests=false build
```
```bash
cd build
```
```bash
meson compile
```
```bash
meson install
```

### Install systemd-dev to install the systemd service files in the right location
```bash
sudo apt install systemd-dev
```

### Install [kmscon](https://github.com/kmscon/kmscon)
1) Download zip from Releases
2) Extract zip
3) Enter kmscon folder
4) Run the following in the kmscon folder:
```bash
meson setup builddir/
```
```bash
meson install -C builddir/
```
**NOTE**: If any missing dependencies show up when installing kmscon, install the appropriate package for your system.

## [tmux](https://github.com/tmux/tmux/wiki) - terminal window splits

### Creating New Window
`Ctrl` + `B` then `C`

### Navigating to Specific Window
`Ctrl` + `B` then `0` / `1` / `2` / `3`

### Kill Window
`Ctrl` + `B` then `&`

### Split Window Horizontal
`Ctrl` + `B` then `"`

### Split Window Vertical
`Ctrl` + `B` then `%`

### Navigating Window Panes
`Ctrl` + `B` then `→` / `↓` / `↑` / `←`

### Kill Window Pane
`Ctrl` + `B` then `X`

## [fresh](https://getfresh.dev/docs/)

### Open IDE at Specific Folder
```bash
fresh your/folder/location/here
```

## [Browsh](https://github.com/browsh-org/browsh)

**IMPORTANT**: [Install Firefox](https://www.omgubuntu.co.uk/2022/04/how-to-install-firefox-deb-apt-ubuntu-22-04)

### Running Browsh
```bash
browsh
```

### Text Only Responses
```bash
curl --header "X-Browsh-Raw-Mode: PLAIN" localhost:4333/https://google.com
```
