

Install i3 using the following:

```
#Install dependencies
sudo apt install -y libxcb1-dev libxcb-keysyms1-dev libpango1.0-dev libxcb-util0-dev libxcb-icccm4-dev libyajl-dev libstartup-notification0-dev libxcb-randr0-dev libev-dev libxcb-cursor-dev libxcb-xinerama0-dev libxcb-xkb-dev libxkbcommon-dev libxkbcommon-x11-dev autoconf libxcb-xrm0 libxcb-xrm-dev automake sudo apt install -y libxcb1-dev libxcb-keysyms1-dev libpango1.0-dev libxcb-util0-dev libxcb-icccm4-dev libyajl-dev libstartup-notification0-dev libxcb-randr0-dev libev-dev libxcb-cursor-dev libxcb-xinerama0-dev libxcb-xkb-dev libxkbcommon-dev libxkbcommon-x11-dev autoconf libxcb-xrm0 libxcb-xrm-dev automake
cd /tmp

# clone the repository
git clone https://www.github.com/Airblader/i3 i3-gaps
cd i3-gaps

# compile & install
autoreconf --force --install
rm -rf build/
mkdir -p build && cd build/

# Disabling sanitizers is important for release versions!
# The prefix and sysconfdir are, obviously, dependent on the distribution.
../configure --prefix=/usr --sysconfdir=/etc --disable-sanitizers
make
sudo make install


#get dmenu, i3 bar status, wmctrl for killing the problematic plasma things
sudo apt install suckless-tools i3-status wmctrl


#Now to create a i3 config so we can add out plasma modifications
i3-config-wizard

```

Now to add the plasma Integration, copy this into your `~/.config/i3/config`

```
# >>> Plasma Integration <<<

# Try to kill the wallpaper set by Plasma (it takes up the entire workspace
# and hides everythiing)
exec --no-startup-id wmctrl -c Plasma
for_window [title="Desktop — Plasma"] kill; floating enable; border none

# Avoid tiling popups, dropdown windows from plasma
for_window [class="plasmashell"] floating enable
for_window [class="Plasma"] floating enable
for_window [class="krunner"] floating enable
for_window [class="Kmix"] floating enable
for_window [class="Klipper"] floating enable
for_window [class="Plasmoidviewer"] floating enable

# >>> Window Rules <<<

# >>> Avoid tiling for non-Plasma stuff <<<
for_window [window_role="pop-up"] floating enable
for_window [window_role="bubble"] floating enable
for_window [window_role="task_dialog"] floating enable
for_window [window_role="Preferences"] floating enable
for_window [window_role="About"] floating enable 
for_window [window_type="dialog"] floating enable
for_window [window_type="menu"] floating enable

# Stop plasma shell popups and notifications from stealing focus
no_focus [class=”plasmashell”]
```

You can disable most of the Desktop Effects in system settings if you like, most are useless with tiling

add the following to your config to restore your wallpaper with nitrogen
```
exec --no-startup-id nitrogen --restore
```


You can install Compton to get transparancy and blur effects
