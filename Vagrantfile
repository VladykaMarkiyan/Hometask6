Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu2204"
  config.vm.boot_timeout = 600


  config.vm.provider "virtualbox" do |vb|
    vb.memory = 2048
    vb.cpus = 2
  end

  config.vm.disk :disk, size: "400MB", name: "extra_storage1"
  config.vm.disk :disk, size: "400MB", name: "extra_storage2"
  config.vm.disk :disk, size: "400MB", name: "extra_storage3"
  config.vm.disk :disk, size: "400MB", name: "extra_storage4"

  config.vm.provision "shell", inline: <<-SHELL
    for disk in /dev/sd{b..e}; do
      sudo parted --script $disk mklabel gpt
      sudo parted --script $disk mkpart primary ext4 0% 100%
    done
  SHELL

  config.vm.provision "shell", inline: <<-SHELL
    for disk in /dev/sd{b..e}1; do
      sudo pvcreate $disk
    done
    sudo vgcreate vg_data /dev/sd{b..e}1
  SHELL

  config.vm.provision "shell", inline: <<-SHELL
    sudo lvcreate -L 790M -n lv1 vg_data
    sudo lvcreate -L 790M -n lv2 vg_data
  SHELL

  config.vm.provision "shell", inline: <<-SHELL
    sudo mkfs.ext4 /dev/vg_data/lv1
    sudo mkfs.ext4 /dev/vg_data/lv2

    sudo mkdir -p /mnt/vol0
    sudo mkdir -p /mnt/vol1

    echo "/dev/vg_data/lv1 /mnt/vol0 ext4 defaults 0 0" | sudo tee -a /etc/fstab
    echo "/dev/vg_data/lv2 /mnt/vol1 ext4 defaults 0 0" | sudo tee -a /etc/fstab

    sudo mount -a
  SHELL
end
