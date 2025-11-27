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

  config.vm.provision "shell", privileged: true, inline: <<-SHELL

    parted --script /dev/sdb mklabel gpt mkpart primary ext4 0% 100%
    parted --script /dev/sdc mklabel gpt mkpart primary ext4 0% 100%
    parted --script /dev/sdd mklabel gpt mkpart primary ext4 0% 100%
    parted --script /dev/sde mklabel gpt mkpart primary ext4 0% 100%

    pvcreate /dev/sdb1
    pvcreate /dev/sdc1
    pvcreate /dev/sdd1
    pvcreate /dev/sde1

    vgcreate vg_data /dev/sdb1 /dev/sdc1 /dev/sdd1 /dev/sde1

    lvcreate -L 790M -n lv1 vg_data
    lvcreate -L 790M -n lv2 vg_data

    mkfs.ext4 /dev/vg_data/lv1
    mkfs.ext4 /dev/vg_data/lv2

    mkdir -p /mnt/vol0
    mkdir -p /mnt/vol1

    echo "/dev/vg_data/lv1 /mnt/vol0 ext4 defaults 0 0" >> /etc/fstab
    echo "/dev/vg_data/lv2 /mnt/vol1 ext4 defaults 0 0" >> /etc/fstab

    mount -a
  SHELL
end
