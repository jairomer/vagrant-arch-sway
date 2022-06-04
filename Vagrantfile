Vagrant.configure("2") do |config|
  # Configure the VM.
  config.vm.box = "archlinux/archlinux"
  config.vm.provider "virtualbox" do |vb|
    vb.gui = true
    vb.memory = 8192
    vb.cpus = 4
    vb.customize ["modifyvm", :id, "--vram", "256"]
    vb.customize ["modifyvm", :id, "--accelerate3d", "on"]
    vb.customize ["modifyvm", :id, "--graphicscontroller", "vmsvga"]
  end

  # Spew some files into place.
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    sudo cp /vagrant/sway.sh /etc/profile.d/sway.sh
    mkdir -p /home/vagrant/.config/sway
    cp /vagrant/sway-config /home/vagrant/.config/sway/config
  SHELL

  # Install paru package manager as an alternative to pacman.
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    sudo pacman --sync --needed --noconfirm base-devel cargo git
    git clone https://aur.archlinux.org/paru.git
    cd paru
    makepkg --syncdeps --install --noconfirm
    cd ../
    rm --recursive --force paru
  SHELL

  # Install various tools.
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    paru --sync --needed --noconfirm \
      moreutils \
      exa \
      bat \
      entr \
      tokei \
      htop \
      podman \
      zsh \
      ripgrep \
      procs \
      fd \
      sd \
      zoxide \
      neovim \
      lazygit

    sudo chsh vagrant --shell /usr/bin/zsh
  SHELL

  # Desktop environment.
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    paru --sync --needed --noconfirm \
      sway \
      gnu-free-fonts \
      polkit \
      wofi \
      mako \
      kitty \
      swaybg \
      swayidle \
      swaylock \
      waybar \
      wl-clipboard \
      otf-font-awesome \
      ly

    sudo systemctl enable ly.service
    sudo systemctl start ly.service
  SHELL

  # brave --ozone-platform=wayland
  # https://github.com/Biont/sway-launcher-desktop
end