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

## configuration.nix

```nix
#nixOS config: 
{
  imports = [
    ./hardware-configuration.nix
  ];

  boot.loader.systemd-boot.enable = false;
  boot.loader.efi.canTouchEfiVariables = false;

  boot.loader.grub.enable = true;
  boot.loader.grub.device = "/dev/sda"; # Assuming your main disk is /dev/sda

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
        command = "${pkgs.tuigreet}/bin/tuigreet --time --cmd Hyprland";
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

  services.xserver.videoDrivers = [ "modesetting" ];

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
    
    ruby_3_5
    crystal_1_17
    shards
    nodejs_24
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
