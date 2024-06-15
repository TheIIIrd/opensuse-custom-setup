# OPENSUSE-CUSTOM-SETUP
One more way to customize opensuse.

<p align="center">
  <img alt="Screenshot 1" src="pictures/screenshot-1.png"/>
  <img alt="Screenshot 2" src="pictures/screenshot-2.png"/>
  <img alt="Screenshot 2" src="pictures/screenshot-3.png"/>
  <img alt="Screenshot 2" src="pictures/screenshot-4.png"/>
</p>


# Working with software

## Repository list configuration
After installation, you should open YaST and configure the software repositories.

- Make sure the local repository of the installation stick is turned off.
- Add the Nvidia and Pacman community repositories.

> `zypper ref` - refreshing a repository means downloading metadata of packages from the medium (if needed).
> 
> `zypper dup` - perform a distribution upgrade. This command applies the state of (specified) repositories onto the system; upgrades (or even downgrades) installed packages to versions found in repositories, removes packages that are no longer in the repositories and pose a dependency problem for the upgrade, handles package splits and renames, etc.
> 
> `zypper ve` - check whether dependencies of installed packages are satisfied. + In case that any dependency problems are found, zypper suggests packages to install or remove to fix them.

```sh
sudo zypper ref 
sudo zypper dup 
sudo zypper ve 
```


## Uninstalling software
By default, after installing the system, the workstation is overloaded with unnecessary programs. It is advisable to uninstall them so that they do not interfere with the use of the system.

- If you have installed KDE Plasma:
```sh
sudo zypper rm kmahjongg kmines kreversi ksudoku akregator kaddressbook konversation kmail kontact ktnef knotes kpat mbox-importer libkdegames kdegames-carddecks-default akonadi-contacts akonadi-import-wizard akonadi-calendar akonadi-calendar-tools opensuse-welcome 
```

- If you have installed GNOME:
```sh
sudo zypper rm gnome-contacts gnome-weather gnome-maps gnome-console gnome-chess gnome-mahjongg gnome-mines gnome-sudoku lightsoff quadrapassel swell-foop iagno evolution polari cheese opensuse-welcome 
```


## Installing useful software
Basic packages that we will need as we work with the system.

> Git wget curl curl aria2 is needed to work with the internet.
> 
> Inxi fastfetch displays useful information about the system.
> 
> Neovim is a powerful text editor.
> 
> Zsh is a modern command shell.
> 
> OBS Package Installer analog of yay/paru.
> 
> java-21-openjdk python311 python311-pip gcc-c++ clang clang-tools are needed to work with Java, Python and C++.
> 
> Dynamic Kernel Module Support is needed to install the Nvidia driver.

```sh
sudo zypper in git wget curl aria2 inxi fastfetch neovim zsh opi java-21-openjdk python311 python311-pip gcc-c++ clang clang-tools dkms 
```

If more than one version of Java is installed, this command will let you select the primary version.
```sh
sudo update-alternatives --config java 
```


## Nvidia graphics card driver installation
Proper (most likely) installation of the Nvidia driver on OpenSUSE with dependencies installed to make the Wayland protocol run smoothly.

> Starting the DKMS daemon. It is obligatory to reboot!
```sh
sudo zypper in dkms 
sudo systemctl enable dkms.service 
```

> After installation, make sure the system is not under load.
> plasma-systemmonitor gnome-system-monitor htop or btop.
```sh
sudo zypper in nvidia-compute-G06 nvidia-compute-G06-32bit nvidia-compute-utils-G06 nvidia-drivers-G06 nvidia-driver-G06-kmp-default nvidia-gl-G06 nvidia-gl-G06-32bit nvidia-utils-G06 nvidia-video-G06 nvidia-video-G06-32bit libnvidia-egl-wayland1 libnvidia-egl-wayland1-32bit 
```

> It is advisable to reboot again and check if the installation is correct using the following commands.
```sh
systemctl status dkms.service 
sudo zypper se nvidia 
nvidia-smi -q 
inxi -G 
```


## Installing additional software

- If you have installed KDE Plasma:
```sh
sudo zypper in torbrowser-launcher inkscape krita gamemode steam vlc partitionmanager 
```

- If you have installed GNOME:
```sh
sudo zypper in torbrowser-launcher inkscape krita gamemode steam vlc gparted gnome-font-viewer 
```


## Virt-manager installation
Virtual machine manager to work with QEMU/KVM.

```sh
sudo zypper in qemu-kvm bridge-utils virt-manager libguestfs guestfs-tools virt-install libvirt-devel libvirt 
```

> For virt-manager to work correctly, it is recommended to add a user to the libvirt group and to configure qemu.conf.
```sh
sudo usermod -G libvirt -a <username> 
```

> You have to change these lines: `user = "<username>"`, `group = "libvirt"`.
```sh
sudo vim /etc/libvirt/qemu.conf 
```

> Running libvirtd background processes.
```sh
sudo systemctl enable libvirtd 
```

## Installing codecs from Packman repositories
You need to play online or offline multimedia content but the content does not want to play or shows errors. Usually this is a sign of [missing codecs](https://en.opensuse.org/SDB:Installing_codecs_from_Packman_repositories): install these packages from Packman to play most music and video:

- ffmpeg
- gstreamer-plugins-good
- gstreamer-plugins-bad
- gstreamer-plugins-ugly
- gstreamer-plugins-libav
- libavcodec
- vlc-codecs

> Opi (Open Build Service Package Installer) works on both Leap and Tumbleweed, and is the easiest way to install community packages and the codecs:

```sh
sudo zypper in opi 
opi codecs 
```


## Adding a flathub repository
Flathub makes it easy to install and update applications for any Linux distribution. Browse popular categories such as performance, graphics, games, and more, or find your favorite application.

> Check for a connected flathub repository. It should be enabled out of the box by default in OpenSUSE.
```sh
flatpak remotes --show-details 
```

> If not, this command will add the flathub repository to the list.
```sh
flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo 
```


## Flatpak application installation

> Installing a theme for GTK3 applications.
```sh
flatpak install org.gtk.Gtk3theme.adw-gtk3 org.gtk.Gtk3theme.adw-gtk3-dark 
```

- If you have installed KDE Plasma
```sh
flatpak install flathub com.github.tchx84.Flatseal page.codeberg.libre_menu_editor.LibreMenuEditor com.github.wwmm.easyeffects me.iepure.devtoolbox com.vysp3r.ProtonPlus com.github.Matoking.protontricks org.onlyoffice.desktopeditors org.kde.kdenlive com.vscodium.codium dev.vencord.Vesktop com.heroicgameslauncher.hgl 
```

- If you have installed GNOME
```sh
flatpak install flathub com.github.tchx84.Flatseal com.mattjakeman.ExtensionManager io.github.realmazharhussain.GdmSettings page.codeberg.libre_menu_editor.LibreMenuEditor com.github.wwmm.easyeffects io.missioncenter.MissionCenter com.raggesilver.BlackBox me.iepure.devtoolbox com.vysp3r.ProtonPlus com.github.Matoking.protontricks org.onlyoffice.desktopeditors org.kde.kdenlive com.vscodium.codium dev.vencord.Vesktop com.heroicgameslauncher.hgl 
```

> If you have problems with vscodium on Wayland session, you may use this command:
```sh
flatpak override --user --nosocket=wayland com.vscodium.codium 
```


## Installing mod launcher for games
[Native launcher](https://github.com/ebkr/r2modmanPlus) for modding games.

> Download the [latest release](https://github.com/ebkr/r2modmanPlus/releases). R2modman.AppImage has the leading version in the name. It can be found in program releases.
```sh
wget https://github.com/ebkr/r2modmanPlus/releases/download/v3.1.48/r2modman-3.1.48.AppImage 
```

> Set permission to run the program.
```sh
chmod +x r2modman-3.1.48.AppImage 
```

> Create desktop shortcut
```sh
nvim .local/share/applications/r2modman.desktop 
```

> Add this to r2modman.desktop. Don't forget to change `/path/to/r2modman.AppImage`!
```
[Desktop Entry]
Name=R2modman
Comment=Infinite mod generator
Exec=/path/to/r2modman.AppImage --no-sandbox
Icon=fuse-emulator
Terminal=false
Type=Application
Categories=Game;
MimeType=x-scheme-handler/ror2mm;
```


## Installing the native engine for the game STALKER
[Engine for the STALKER](https://github.com/OpenXRay/xray-16) series of games. Makes the game native.

> Download the necessary dependencies.
```sh
sudo zypper in git cmake make gcc gcc-c++ glew-devel openal-devel cryptopp-devel libogg-devel libtheora-devel libvorbis-devel SDL2-devel lzo-devel libjpeg-turbo-devel 
```

> To get the source code, run:
```sh
git clone https://github.com/OpenXRay/xray-16.git --recurse-submodules 
```

> Enter the repository clone and create a building directory there. The name of the directory doesn't matter, e.g. bin or build (bin will be used for the rest of this documentation to refer to this directory). An example for creating the directory and entering it for the terminal:
```sh
cd xray-16 && mkdir bin && cd bin 
```

> Once you're inside of bin, configure the project by running:
```sh
cmake .. -DCMAKE_INSTALL_LIBDIR=lib64 -DCMAKE_INSTALL_PREFIX=/usr 
```

> To compile the engine, run:
```sh
make -jx 
```

> ! x denoted the number of threads you want to assign to the compile process. For example, if you want to use 4 threads:
```sh
make -j4 
```

> To make the engine, run:
```
QA_RPATHS=$(( 0x0001|0x0010 )) make package 
```

- To install via zypper, run:
```sh
sudo zypper in /path/to/<package_name>.rpm 
```

- To install via rpm, run:
```
sudo rpm -ivh /path/to/<package_name>.rpm 
```


## Installing the open source version of the Minecraft Launcher
[Launcher](https://llaun.ch) with open source code and convenient settings.

> Download the [latest release](https://llaun.ch/jar).
> Alternative sources [one](https://llaun.ch) or [two](https://lln4.ru).

> Create desktop shortcut.
```sh
nvim ~/.local/share/applications/minecraft.desktop 
```

> Add this to minecraft.desktop. Don't forget to change `/path/to/LegacyLauncher_legacy.jar`!
```
[Desktop Entry]
Name=Minecraft
Comment=Infinite idea generator
Exec=gamemoderun java -jar /path/to/LegacyLauncher_legacy.jar
Icon=minecraft
Terminal=false
Type=Application
Categories=Game;
```


## Fan control on MSI notebooks

The application requires the `ec_sys` module with option `write_support=1` to run. If the `ec_sys` kernel module is not included in your distribution's kernel, you can use the `acpi_ec` kernel module.

> Checking for the ec_sys module:
```sh
sudo modinfo ec_sys 
```

> Create ec_sys.conf:
```sh
sudo vim /etc/modules-load.d/ec_sys.conf 
```

> Add this to ec_sys.conf:
```
# Load ec_sys at boot
ec_sys write_support=1
```

> Download `MControlCenter-x.x.x.tar.gz` from the [releases](https://github.com/dmitry-s93/MControlCenter/releases) page.
```sh
wget https://github.com/dmitry-s93/MControlCenter/releases/download/0.4.1/MControlCenter-0.4.1-bin.tar.gz 
```

> Unpack the archive with the program.
```sh
tar -xvf MControlCenter-0.4.1-bin.tar.gz 
```

> Open terminal in unpacked directory. Run the script.
```sh
sudo ./install 
```


## Zypper configuration
After installing the required packages, it is desirable to disable the installation of recommended packages by default so that the system remains clean when upgrading.

> Normally this parameter is on line 403 `solver.onlyRequires = true`.
```sh
sudo vim /etc/zypp/zypp.conf 
```


## System integrity verification

- Verify rpm packages:
```sh
sudo zypper ref 
sudo zypper dup 
sudo zypper ve 
```

- Verify flatpak packages:
```sh
flatpak update 
flatpak uninstall --unused 
flatpak repair 
```


# System customization

## Icon setup
> To make icons available to all users of the system, instead of `./install.sh standard`, run `sudo ./install.sh standard`.

> Traditional icons:
```sh
git clone https://github.com/vinceliuice/Tela-icon-theme.git 
cd Tela-icon-theme 
./install.sh standard 
```

> Rounded icons
```sh
git clone https://github.com/vinceliuice/Tela-circle-icon-theme.git 
cd Tela-circle-icon-theme 
./install.sh standard 
```

> If you want to change some icons
```sh
cat /var/lib/flatpak/exports/share/applications/com.github.Matoking.protontricks.desktop 
nvim .local/share/applications/com.github.Matoking.protontricks.desktop 
Icon=katomic

cat /var/lib/flatpak/exports/share/applications/dev.vencord.Vesktop.desktop 
nvim .local/share/applications/dev.vencord.Vesktop.desktop 
Icon=discord

cat /var/lib/flatpak/exports/share/applications/me.iepure.devtoolbox.desktop 
nvim .local/share/applications/me.iepure.devtoolbox.desktop 
Icon=via

cat /var/lib/flatpak/exports/share/applications/page.codeberg.libre_menu_editor.LibreMenuEditor.desktop 
nvim .local/share/applications/page.codeberg.libre_menu_editor.LibreMenuEditor.desktop 
Icon=org.gnome.Tecla.svg
```


## Installing the unofficial GTK3 port of libadwaita

> Install theme from [this](https://github.com/lassekongo83/adw-gtk3) repo.
```sh
wget https://github.com/lassekongo83/adw-gtk3/releases/download/v5.3/adw-gtk3v5.3.tar.xz 
```

> Unzip the archive
```sh
tar -xvf adw-gtk3v5.3.tar.xz 
```

> Install theme
```sh
sudo cp -r adw-gtk3 /usr/share/themes/ 
sudo cp -r adw-gtk3-dark /usr/share/themes/ 
```

> Add flatpak applications theme support
```sh
flatpak install org.gtk.Gtk3theme.adw-gtk3 org.gtk.Gtk3theme.adw-gtk3-dark 
```

## GNOME extensions

```
# changing keyboard layout without a popup window
quick-lang-switch@ankostis.gmail.com

# application icon running in the background
appindicatorsupport@rgcjonas.gmail.com

# weather display for your region
openweather-extension@jenslody.de

# Gnome Shell interface blur
blur-my-shell@aunetx

# clipboard manager for Gnome Shell
clipboard-indicator@tudmotu.com

# sleep mode on/off
caffeine@patapon.info

# system performance monitoring
Vitals@CoreCoding.com

# advanced Gnome Shell settings
just-perfection-desktop@just-perfection
```


## ZSH installation and configuration
Install zsh and run it.

> Zsh will ask you to configure it after the first run.
> Look at all the items and click 0 in each if you are satisfied.
```sh
sudo zypper in zsh 
```

> To run
```sh
zsh 
```

> This font supports icons.
> It is necessary for correct output of information in the terminal.
```sh
wget https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Regular.ttf 
```


## Oh-my-zsh framework
Oh-my-zsh will extend the capabilities of regular zsh.

```sh
sh -c "$(wget https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)" 
```


## Plugins for zsh
These extensions will provide hints while using zsh.

```sh
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting 
git clone https://github.com/zsh-users/zsh-autosuggestions.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions 
```

> After installing the zsh extensions, you should put them in `.zshrc`.
> 
> Open the config and find the line `plugins=(git)`.
> 
> It should be changed to `plugins=(git zsh-autosuggestions zsh-syntax-highlighting)`.
```sh
nvim .zshrc 
```

> Restart the config
```sh
source .zshrc 
```


## P10K theme
P10k will make zsh more beautiful.

```sh
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k 
```

> After installing the 10k, you should put them in `.zshrc`.
> 
> You must replace the value of ZSH_THEME with `ZSH_THEME="powerlevel10k/powerlevel10k"`.
```sh
nvim .zshrc 
```

> Restart the config
```sh
source .zshrc 
```
