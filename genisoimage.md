# brand new install with iso
virt-install -n gues_name --os-type=Linux --ram=2048 --vcpus=2 --disk
path=/path/to/disk/new_guest_disk.img,size=20 --cdrom=/path/tp/ISOs/new_iso.iso
--network bridge:br0 --graphics vnc,listen=0.0.0.0 --noautoconsole

# delete kvm
virsh list --all
virsh undefine guest_name
virsh vol-list --pool Disks # Disks is where I have my disks stored,
/path/to/Disks
virsh vol-delete --pool Disks guest_disk.img

# import from existing image
virt-install --name guest_name --memory 2048 --disk
/path/to/Disks/ubuntu-15.04-snappy-amd64+generic.img --network bridge:br0
--graphics vnc,listen=0.0.0.0 --noautoconsole --import

# setting password for Fedora cloud qcow2 image
yum install libguestfs-tools-c
virt-customize --add kvm_disk_to_import.qcow2 --root-password password:
r3@11ys3cur3
virt-customize --add kvm_disk_to_import.qcow2 --run-command "systemctl mask
cloud-init.service" #cloud-init can sometime hang and this disables it during
bootup

# downloading images using virt-builder
virt-builder --list # to see the available distro images.
virt-builder distro_image --root-password password:r3@11ys3cur3
