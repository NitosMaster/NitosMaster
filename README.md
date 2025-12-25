## Hello

🇵🇹 / ♂️ / 2️⃣3️⃣ / Mart

**Interests**: 
* Comp Sci
* Math
* Physics(The weird kind)
* Politics(Ancap) and Philosophy(Rationalist Nitzschean)
* Existancialist music and films

## Favorite things

**Favorite Philophers**:
* Friedrich Nietzsche 
* Max Stirner(I litteraly just find him funny that's all)
* Jean Paul Sartre
* Thomas Aquinas(I like how he argues, but don't agree with him)

**Favorite Langs**:
* ![Crystal](https://img.shields.io/badge/language-crystal-black)
* ![Python](https://img.shields.io/badge/language-python-yellow)
* ![CSS](https://img.shields.io/badge/language-css-purple)
* ![ASM](https://img.shields.io/badge/language-asm-green)(it's fun to try to do simple things in the most painful way possible)
* ![nix](https://img.shields.io/badge/language-nix-blue) (I use nixOS, btw.)

**Favorite Albums**:
* *loveless* by *my bloody valentine*
* **OK Computer** by Radiohead
* *The Dark Side of the Moon* by Pink Floyd
* **Madvillain** by Madvillainy

**Favorite historical periods**:
* **The French Revolution**
* **Medieval Fighting Techniques**
* **Modern history**

**Favorite Videogames**:
* **Garry's Mod**
* **TF2**
* **Speedrun-esques** such as Hitman: WoA, UltraKill, Neon White, Quake 1, etc
* **Hard games(pretty broad ik)** such as Sekiro, Hollow Knight, Dead Cells, etc

**Favorite Foods**:
* **Breaded**
* **Pizza**
* **Hamburgers**
* <ins>**_You_**<ins>

## dotfiles

### nixOS config

```nix
{
  imports = [
    ./hardware-configuration.nix
  ];

  boot.loader.systemd-boot.enable = false;
  boot.loader.efi.canTouchEfiVariables = true;
  boot.loader.grub.enable = true;
  boot.loader.grub.device = "nodev";
  boot.loader.grub.efiSupport = true;
  boot.loader.grub.useOSProber = true;

  networking.hostName = "nitosmaster-nixos";
  networking.networkmanager.enable = true;

  time.timeZone = "Europe/Lisbon";

  i18n.defaultLocale = "en_US.UTF-8";
  i18n.extraLocaleSettings = {
    LC_ADDRESS = "pt_PT.UTF-8";
    LC_IDENTIFICATION = "pt_PT.UTF-8";
    LC_MEASUREMENT = "pt_PT.UTF-8";
    LC_MONETARY = "pt_PT.UTF-8";
    LC_NAME = "pt_PT.UTF-8";
    LC_NUMERIC = "pt_PT.UTF-8";
    LC_PAPER = "pt_PT.UTF-8";
    LC_TELEPHONE = "pt_PT.UTF-8";
    LC_TIME = "pt_PT.UTF-8";
  };

  services.xserver.xkb.layout = "pt";
  console.keyMap = "pt-latin1";

  programs.hyprland = {
    enable = true;
    xwayland.enable = true;
  };

  services.greetd = {
    enable = true;
    settings = {
      default_session = {
        command = "${pkgs.greetd.tuigreet}/bin/tuigreet --time --cmd Hyprland";
        user = "greeter";
      };
    };
  };

  xdg.portal = {
    enable = true;
    extraPortals = [ pkgs.xdg-desktop-portal-gtk ];
    config.common.default = "*";
  };

  security.rtkit.enable = true;
  services.pipewire = {
    enable = true;
    alsa.enable = true;
    alsa.support32Bit = true;
    pulse.enable = true;
    jack.enable = true;
  };

  programs.steam = {
    enable = true;
    remotePlay.openFirewall = true;
    dedicatedServer.openFirewall = true;
  };

  hardware.graphics = {
    enable = true;
    enable32Bit = true;
  };

services.xserver.videoDrivers = [ "nvidia" ];

  hardware.nvidia = {
    modesetting.enable = true;
    open = false;
    nvidiaSettings = true;
    package = config.boot.kernelPackages.nvidiaPackages.stable;
  };

services.ollama = {
    enable = true;
    acceleration = "cuda"; # Uses NVIDIA GPU
    environmentVariables = {
      OLLAMA_ORIGINS = "https://nitosmaster.neocities.org";
    };
  };

  users.users.nitosmaster = {
    isNormalUser = true;
    description = "Nitosmaster";
    extraGroups = [ "networkmanager" "wheel" "video" "audio" "input" ];
    shell = pkgs.zsh;
  };

  programs.zsh.enable = true;

  environment.systemPackages = with pkgs; [
    cloudflared
    waybar
    dunst
    rofi
    swww
    hyprpaper
    wlogout
    grim
    slurp
    wl-clipboard
    cliphist
    kitty
    alacritty
    starship

    # Corrected Nerd Fonts attribute names
    pkgs.nerd-fonts.jetbrains-mono
    pkgs.nerd-fonts.fira-code
    pkgs.nerd-fonts.meslo-lg
    
    eza
    bat
    fzf
    zoxide
    ripgrep
    fd
    neovim
    lua-language-server
    nil
    black
    stylua
    nixpkgs-fmt
    firefox
    
    python311
    python311Packages.pip
    python311Packages.virtualenv
    
    ruby
    crystal
    shards
    nodejs
    gcc
    gnumake
    cmake
    git
    gh
    lazygit
    networkmanagerapplet
    pavucontrol
    brightnessctl
    playerctl
    
    pkgs.xfce.thunar
    ranger
    btop
    htop
    cava
    fastfetch
    neofetch
    hyfetch
    
    noto-fonts
    noto-fonts-color-emoji
    wget
    curl
    unzip
    zip
    tree
  ];

  fonts.packages = with pkgs; [
    pkgs.nerd-fonts.jetbrains-mono
    pkgs.nerd-fonts.fira-code
    pkgs.nerd-fonts.meslo-lg
    
    noto-fonts
    noto-fonts-color-emoji
    font-awesome
  ];

  fonts.fontconfig = {
    enable = true;
    defaultFonts = {
      monospace = [ "JetBrainsMono Nerd Font" ];
      sansSerif = [ "Noto Sans" ];
      serif = [ "Noto Serif" ];
      emoji = [ "Noto Color Emoji" ];
    };
  };

  environment.sessionVariables = {
    NIXOS_OZONE_WL = "1";
    EDITOR = "nvim";
    VISUAL = "nvim";
  };

  nix.settings.experimental-features = [ "nix-command" "flakes" ];

  nix.gc = {
    automatic = true;
    dates = "weekly";
    options = "--delete-older-than 7d";
  };

  nixpkgs.config.allowUnfree = true;

  hardware.bluetooth.enable = true;
  services.blueman.enable = true;

  services.printing.enable = true;

  system.stateVersion = "24.05";
}
```

### ~/.config/hypr/hyprland.conf

```conf
monitor=,highres,auto,1

env = LIBVA_DRIVER_NAME,nvidia
env = XDG_SESSION_TYPE,wayland
env = GBM_BACKEND,nvidia-drm
env = __GLX_VENDOR_LIBRARY_NAME,nvidia
env = NIXOS_OZONE_WL,1

input {
    kb_layout = pt
    follow_mouse = 1
}

general {
    gaps_in = 5
    gaps_out = 10
    border_size = 2
    col.active_border = rgb(4baf4b) rgb(af64af) 45deg
    col.inactive_border = rgba(595959aa)
    layout = dwindle
}

$mainMod = SUPER

bind = $mainMod, Q, exec, kitty
bind = $mainMod, C, killactive,
bind = $mainMod, M, exit,
bind = $mainMod, E, exec, thunar
bind = $mainMod, R, exec, rofi -show drun
bind = $mainMod, V, togglefloating,

bind = $mainMod, left, movefocus, l
bind = $mainMod, right, movefocus, r
bind = $mainMod, up, movefocus, u
bind = $mainMod, down, movefocus, d

bind = $mainMod, 1, workspace, 1
bind = $mainMod, 2, workspace, 2
bind = $mainMod, 3, workspace, 3

bindm = $mainMod, mouse:272, movewindow
bindm = $mainMod, mouse:273, resizewindow

exec-once = waybar &
```

### ~/.config/waybar/config

```conf
{
    "layer": "top",
    "position": "top",
    "height": 30,
    "modules-left": ["hyprland/workspaces", "hyprland/window"],
    "modules-center": ["clock"],
    "modules-right": ["cpu", "memory", "tray"],

    "hyprland/workspaces": {
        "format": "{name}",
        "on-click": "activate"
    },
    "clock": {
        "format": "{:%H:%M:%S [SYSTEM_TIME]}",
        "interval": 1,
        "tooltip-format": "<big>{:%Y %B}</big>\n<tt><small>{calendar}</small></tt>"
    },
    "cpu": {
        "format": "CPU: {usage}%",
        "interval": 2
    },
    "memory": {
        "format": "RAM: {}%"
    }
}
```

### ~/.config/waybar/style.css
```css
* {
    border: none;
    font-family: "quake", "JetBrainsMono Nerd Font";
    font-size: 14px;
}

window#waybar {
    background-color: #282323;
    border-bottom: 3px double #4baf4b;
    color: #af64af;
}

#workspaces button {
    padding: 0 5px;
    color: #4baf4b;
}

#workspaces button.active {
    color: #af64af;
    text-shadow: 0 0 5px #af64af;
    border-bottom: 2px solid #af64af;
}

#clock, #cpu, #memory, #network, #window {
    padding: 0 15px;
    border-left: 1px dotted #4baf4b;
}

#clock {
    color: #4baf4b;
    text-shadow: 0 0 5px #4baf4b;
}

#window {
    border: none;
    font-style: italic;
}
```