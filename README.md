# Things to do to set up new Linux install
Operating System: Kubuntu 22.10
KDE Plasma Version: 5.25.5
KDE Frameworks Version: 5.98.0
Kernel Version: 5.19.0-23-generic (64-bit)

## Fix Nvidia GPU + KWIN multiple monitor refresh rate problem
Copy `environment` to `/etc/environment` (requires `sudo`):

```
sudo cp environment /etc/environment
```

NOTE: May want to include `__GL_SYNC_TO_VBLANK=0` to reduce input lag. Needs testing. 

### Reference Material
- https://www.reddit.com/r/kde/comments/w96t76/compositor_still_caps_144hz60hz_dualmonitor_setup/
- https://www.reddit.com/r/linux_gaming/comments/pon1j6/vblank_mode0_not_doing_anything/

## GreenWithEnvy to set GPU Fan Curve
GWE webpage: https://gitlab.com/leinardi/gwe

### Quick Install
```
flatpak --user remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak --user install flathub com.leinardi.gwe
flatpak update
flatpak run com.leinardi.gwe
```

### My Fan Curve for Zotac RTX 3090 OC Trinity
| Temperature | Duty |
| ----------- | ---- |
| 40          | 46   |
| 55          | 50   |
| 65          | 60   |
| 75          | 70   |
| 85          | 85   |
| 90          | 100  |

## Undervolt the GPU
`cd` into the `gpu-undervolt` directory here, then:

```
chmod +x gpu_undervolt.sh
sudo ./gpu_undervolt.sh
```

### Set up to run on system start
In `gpu-undervolt.service`, edit the `ExecStart` path to point to the `gpu_undervolt.sh` script. Then,

```
sudo cp gpu-undervolt.service /etc/systemd/system/.
sudo systemctl daemon-reload
sudo systemctl enable /etc/systemd/system/gpu-undervolt.service
```

### Reference Material
- https://github.com/NVIDIA/open-gpu-kernel-modules/discussions/236
- https://github.com/xor2k/gpu_undervolt
- NOTE: My script is updated compared to the one above

## Kitty Terminal Emulator
Copy the entire folder (`kitty`), which contains 2 files, to `/home/username/.config/`:

```
cp -r kitty /home/username/.config/.
```

### Reference Material
- https://sw.kovidgoyal.net/kitty/

## Firefox Customization
Go to `about:config` in the browser and set `toolkit.legacyUserProfileCustomizations.stylesheets` to `true`. 

Then, go to `about:support` in the browser and copy the `Profile Directory` path. Mine was:

`/home/nigel/.mozilla/firefox/n7w2w76p.default-release-1`

```
cd /path/to/your/profile/directory
git clone https://github.com/MrOtherGuy/firefox-csshacks.git chrome
```

Then, `cd` back here, and copy the contents of `firefox` into the above directory:

```
cp firefox/* /path/to/your/profile/directory/chrome/.
```

### Reference Material
- https://github.com/MrOtherGuy/firefox-csshacks


## VSCode Keychain Popup
Whenever you start VSCode for the first time after starting the computer, you'll get a popup about keychain access and have to enter your `sudo` password. 

```
sudo cp sddm /etc/pam.d/.
```

## Overwatch 2
1. Install Lutris (if not installed)
2. Install Wine dependencies for Ubuntu:

```
sudo dpkg --add-architecture i386 && sudo apt update && sudo apt install -y wine64 wine32 libasound2-plugins:i386 libsdl2-2.0-0:i386 libdbus-1-3:i386 libsqlite3-0:i386
```

3. Install Vulkan and arch i386 dependencies:

```
sudo dpkg --add-architecture i386 && sudo apt update && sudo apt install -y libvulkan1 libvulkan1:i386
```

4. Install Battle.net by clicking the `+` in the upper left of Lutris and searching `battle.net`
5. Change Runner options Wine version to `overwatch2-caffe-7.18-x86_64 (default)` in Lutris by clicking on Battle.net, and at the bottom, clicking the up arrow next to `Play`, selecting `Configure`, going to the `Runner options` tab, and changing the `Wine version` via the dropdown menu.
6. Download pre-compiled shaders from https://github.com/GolDNenex/overwatch2-dxvk-cache

```
cp ~/Downloads/Overwatch.dxvk-cache ~/Games/battlenet/.
```

7. (Maybe) Set `__GL_SHADER_DISK_CACHE_SKIP_CLEANUP=1` in Lutris by clicking on Battle.net, and at the bottom, clicking the up arrow next to `Play`, selecting `Configure`, going to the `System options` tab, and adding it to `Environment variables`.

### Reference Material
- https://github.com/lutris/docs/blob/master/Battle.Net.md
- https://github.com/lutris/docs/blob/master/Overwatch.md
- https://www.reddit.com/r/linux_gaming/comments/y41tbp/overwatch_2_non_persistent_shader_cache/

## Zsh/Oh-My-Zsh/Powerlevel10k
```
sudo apt install zsh
chsh -s $(which zsh)
```

Close all Terminal windows, reopen, then you'll be prompted to create a `.zshrc` file. Select Option `0`, because all will be added in the next steps.

### Install Font for P10k
https://github.com/romkatv/powerlevel10k#manual-font-installation

Then,

```
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

Close all Terminal windows, reopen, and customize according to the prompts. 

### Reference Materials
- https://ohmyz.sh/
- https://github.com/romkatv/powerlevel10k#oh-my-zsh