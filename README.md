# To use

1. Install Vagrant

# Install parallels tools

## On mac first:
    
VM_ID=$(prlctl list -a | grep running | awk '{print $1}' | head -n 1) && ISO_PATH=$(if [[ $(uname -m) == "x86_64" ]]; then echo "/Applications/Parallels Desktop.app/Contents/Resources/Tools/prl-tools-lin.iso"; elif [[ $(uname -m) == "arm64" ]]; then echo "/Applications/Parallels Desktop.app/Contents/Resources/Tools/prl-tools-lin-arm.iso"; fi) && prlctl set $VM_ID --device-set cdrom0 --image "$ISO_PATH" --connect

## In VM:
sudo mkdir -p /media/cdrom0; if [[ -f /etc/fedora-release ]]; then sudo dnf install -y gcc kernel-devel-$(uname -r) kernel-headers-$(uname -r) make checkpolicy selinux-policy-devel; sudo mount -o exec /dev/sr0 /media/cdrom0; cd /media/cdrom0; sudo ./install; elif [[ -f /etc/debian_version ]] || [[ -f /etc/kali_version ]] || [[ -f /etc/os-release && $(grep -Ei "NAME=\"Debian|NAME=\"Ubuntu" /etc/os-release) ]]; then sudo apt-get install -y dkms libelf-dev linux-headers-$(uname -r) build-essential; sudo mount -o exec /dev/sr0 /media/cdrom0; cd /media/cdrom0; sudo ./install; fi

# In the rails app

1. vim config/environments/development.rb config.hosts << "http://openstep.ddns.net"
2. bundle install --path vendor/bundle/