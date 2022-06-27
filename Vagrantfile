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

  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    set -eux

    # Put local files into place in the VM.
    sudo cp /vagrant/sway.sh /etc/profile.d/sway.sh
    cp -r /vagrant/dotfiles /home/vagrant/dotfiles

    # Install paru package manager as an alternative to pacman.
    sudo pacman --sync --refresh --needed --noconfirm base-devel cargo git
    git clone https://aur.archlinux.org/paru.git
    cd paru
    makepkg --syncdeps --install --noconfirm
    cd ../
    rm --recursive --force paru

    # Install tons of packages!
    paru --sync --refresh --needed --noconfirm \
      moreutils \
      python3 \
      exa \
      bat \
      entr \
      tokei \
      htop \
      podman \
      podman-dnsname \
      docker-compose \
      buildah \
      skopeo \
      zsh \
      ripgrep \
      procs \
      fd \
      sd \
      zoxide \
      neovim \
      lazygit \
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
      ly \
      fish \
      qutebrowser \
      pipewire-jack \
      wireplumber \
      qt5-wayland \
      stow \
      tmux \
      ttc-iosevka \
      ttf-nerd-fonts-symbols \
      sway-launcher-desktop

    # Set default shell to fish.
    sudo chsh vagrant --shell /usr/bin/fish

    # Enable ly (TUI-based login manager.)
    sudo systemctl enable ly.service
    sudo systemctl start ly.service

    # Set up dotfile symlinks.
    cd /home/vagrant/dotfiles
    stow -v */

    # Configure rootless podman for vagrant user.
    sudo touch /etc/subuid /etc/subgid
    sudo usermod --add-subuids 100000-165535 --add-subgids 100000-165535 vagrant
    podman system migrate
    systemctl --user enable podman.service
    systemctl --user start podman.service

    # Configure my customized VIM IDE environment
    git clone https://github.com/jairomer/neovim-config.git $HOME/.config/nvim \
    && cd $HOME/.config/nvim \
    && git submodule update --init --recursive \
    && cd $HOME/.config/nvim \
    && python3 -m pip install --user --upgrade pynvim \
    && python bundle/YouCompleteMe/install.py

  SHELL

  # IN PROGRESS
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    paru --sync --refresh --needed --noconfirm \
      gammastep \
      direnv \
      starship

    paru --sync --refresh --needed --noconfirm \
      nvm
    source /usr/share/nvm/init-nvm.sh
    nvm install node
  SHELL

end
