{ config, pkgs, ... }:

{
  imports = [ ./hardware-configuration.nix ];

  # BOOT
  boot.loader.systemd-boot.enable = true;
  boot.loader.efi.canTouchEfiVariables = true;

  # NETWORK
  networking.hostName = "nixos";
  networking.networkmanager.enable = true;

  # TIME + LOCALE
  time.timeZone = "America/Bogota";
  i18n.defaultLocale = "es_CO.UTF-8";

  # USER
  users.users.mj = {
    isNormalUser = true;
    description = "mj";
    extraGroups = [ "wheel" "networkmanager" "video" "audio" ];
  };

  # NIRI
  programs.niri.enable = true;
  services.displayManager.gdm.enable = true;

  # XDG portal (importante en Wayland)
  xdg.portal = {
    enable = true;
    extraPortals = [ pkgs.xdg-desktop-portal-gtk ];
  };

  # PAQUETES útiles para Niri
  environment.systemPackages = with pkgs; [
    kitty
    waybar
    rofi-wayland
    dunst
    wl-clipboard
    grim
    slurp
  ];

  nixpkgs.config.allowUnfree = true;

  # AUDIO
  services.pipewire = {
    enable = true;
    pulse.enable = true;
    alsa.enable = true;
    alsa.support32Bit = true;
  };
  hardware.pulseaudio.enable = false;

  # AMD
  hardware.graphics.enable = true;
  hardware.graphics.enable32Bit = true;
  services.xserver.videoDrivers = [ "amdgpu" ];

  system.stateVersion = "24.11";
}