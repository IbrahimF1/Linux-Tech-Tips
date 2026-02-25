# Linux Tech Tips

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
