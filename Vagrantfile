Vagrant.configure("2") do |config|
  config.vm.box = "spox/ubuntu-arm"
  config.vm.box_version = "1.0.0"
  config.vm.network "private_network", ip: "192.168.56.11"

  config.vm.provider "vmware_desktop" do |vmware|
    vmware.gui = true
    vmware.allowlist_verified = true
    vmware.memory = "2048"
  end

  config.vm.provision "shell", inline: <<-SHELL
    set -eux

    echo "Updating package information..."
    sudo apt-get update

    echo "Installing prerequisites..."
    sudo apt-get install -y ca-certificates curl

    echo "Creating directory for Docker keyrings..."
    sudo install -m 0755 -d /etc/apt/keyrings

    echo "Downloading Docker GPG key..."
    sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc

    echo "Setting permissions for Docker GPG key..."
    sudo chmod a+r /etc/apt/keyrings/docker.asc

    echo "Adding Docker repository..."
    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
    $(. /etc/os-release && echo \"$VERSION_CODENAME\") stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

    echo "Updating package information again..."
    sudo apt-get update

    echo "Installing Docker packages..."
    sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

    echo "Docker installation complete."
  SHELL
end

