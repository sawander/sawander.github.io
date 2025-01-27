# Install
apt-get install libguestfs-tool virtinst

TARGET_DISK="~/fedora/CentOS-Stream-GenericCloud-x86_64-10-latest.x86_64.qcow2"
PASSWORD="fedora"
HOSTNAME="

# add root password
virt-customize --add "PathToTagetDisk" \
--root-password password:MyRootPW \
--hostname "VMName" \ 
--firstboot-command 'ssh-keygen -A && systemctl restart sshd'


# enable ssh login
virt-customize --add "PathToTagetDisk" \
--root-password password:MyRootPW \
--hostname "VMName" \ 
--firstboot-command 'ssh-keygen -A && systemctl restart sshd'


# Create Debian Cloud image
virt-customize --add "PathToTagetDisk" \
--root-password password:MyRootPW \
--hostname "VMName" \ 
--firstboot-command 'ssh-keygen -A && systemctl restart sshd'


# Creating Cloud-Init Configuration Files

# meta-data
instance-id: hal9000
local-hostname: hal9000


# user-data
users:
  - name: sumit
    ssh_authorized_keys:
      - ssh-rsa
        AAAAB3NzaC1yc2EAAAADAQABAAABAQDDzMSSuEky2HWKy3/p01BXURLkYhDkJ+Wpd45kU7s0737LXx9zhRqWyX0pnUcGf1A5uKpy6JiaHNjRT/PBKMye0ej1CSurPZXOEyjSSK4MlW8NkRAHiLBuBAhetG3jANWKxcvsvsp172XdK8yP81B0w4qlKQz7J5GbALuwSwFEQu01tjf4aErEvV8xXxl2y1O8DMxjTiXT2WLTeoUDQldBm3m56ogtajnJz7USiZPePUZHcm6DMp9/2+ucef3/1AAtK0adQzwhnj6W+0eCTqdQz+DF9erqsMkd7QoRaQ0/ZK/rqljMwdbux6NySA1U5Zx2JaNUlClmfqxlkBm8TbY7
sumit@sumit-ghosh.com
    sudo: ["ALL=(ALL) NOPASSWD:ALL"]
    groups: sudo
    shell: /bin/bash



#  Create Volume cidata
$ genisoimage -output cidata.iso -V cidata -r -J user-data meta-data
I: -input-charset not specified, using utf-8 (detected in locale settings)
Total translation table size: 0
Total rockridge attributes bytes: 331
Total directory bytes: 0
Path table size(bytes): 10
Max brk space used 0
183 extents written (0 MB)


# Create vm image
virt-install --name=hal9000 --ram=2048 --vcpus=1 --import --disk
path=hal9000.img,format=qcow2 --disk path=cidata.iso,device=cdrom
--os-variant=ubuntu20.04 --network bridge=virbr0,model=virtio --graphics vnc,
listen=0.0.0.0 --noautoconsole



