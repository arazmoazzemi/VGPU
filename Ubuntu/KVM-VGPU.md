## UBUNTU-22.04 VGP Unlock

- ### [Alternatives](https://www.libhunt.com/r/vgpu_unlock)

- LibVF.IO
----
- ***https://docs.google.com/document/d/1pzrWJ9h-zANCtyqRgS7Vzla0Y8Ea2-5z2HEi4X75d2Q/edit***

- ***https://www.youtube.com/watch?v=cPrOoeMxzu0***

- ***https://wvthoog.nl/proxmox-7-vgpu-v2/***

- ***https://gitlab.com/polloloco/vgpu-proxmox***

----

- VGPU_Installation
```bash
nano /etc/apt/sources.list
deb http://download.proxmox.com/debian/pve buster pve-no-subscription
deb http://download.proxmox.com/debian/pve bullseye pve-no-subscription

apt-get upupdate -y
apt-get	upgrade -y
apt -y install python3 python3-pip git build-essential pve-headers dkms jq
apt install -y git build-essential pve-headers-`uname -r` dkms jq cargo mdevctl unzip uuid
pip install frida

cd /etc/opt
git clone https://github.com/DualCoder/vgpu_unlock.git

# wget http://ftp.br.debian.org/debian/pool/main/mdevctl/mdevctl_0.78-l_all.deb
wget http://ftp.br.debian.org/debian/pool/main/m/mdevctl/mdevctl_0.81-1_all.deb

chmod -R +x vgpu_unlock

dpkg -i mdevctl_0.81-1_all.deb
```
----

- ### Enable_IOMMU


nano /etc/default/grub

disable or comment below line
GRUB_CMDLINE_LINUX_DEFAULT="quiet"

Add below lines :

#GRUB_CMDLINE_LINUX_DEFAULT="quiet amd_iommu=on iommu=pt"
GRUB_CMDLINE_LINUX_DEFAULT="quiet intel_iommu=on iommu=pt"


update-grub



--------------------------------
# load_VFIO_Modules
# add below lines


nano /etc/modules

vfio
vfio_iommu_type1
vfio_pci
vfio_virqfd

---------------------------------------------------
echo "options vfio_iommu_type1 allow_unsafe_interupts=1" > /etc/modprobe.d/iommu_unsafe_interrupts.conf
echo "options kvm ignore_msrs=1" > /etc/modprobe.d/kvm.conf
echo "blacklist nouveau" >> /etc/modprobe.d/blacklist.conf

update-initramfs -u

reboot

----------------------------------------------------

dmesg | grep -e DMAR -e IOMMU

--------Download nvidia kvm driver -----------------install proxmox 6.4

# 450.80.20        11.1 Linux KVM Vgpu version

# copy NVIDIA-Linux-x86_64-450.80-vgpu-kvm to server

chmod +x NVIDIA-Linux-x86_64-450.80-vgpu-kvm.run
./NVIDIA-Linux-x86_64-450.80-vgpu-kvm.run --dkms

--------------------------------------------------------

nano /usr/lib/systemd/system/nvidia-vgpud.service

#comment below line
#ExecStart=/usr/bin/nvidia-vgpud

#add below line above it
ExecStart=/root/vgpu_unlock/vgpu_unlock /bin/nvidia-vgpud


nano /usr/lib/nvidia/systemd/nvidia-vgpud.service

#comment below line
#ExecStart=/usr/bin/nvidia-vgpud

#add below line above it
ExecStart=/root/vgpu_unlock/vgpu_unlock /bin/nvidia-vgpud



nano /usr/lib/systemd/system/nvidia-vgpu-mgr.service

#comment below line
#ExecStart=/usr/bin/nvidia-vgpu-mgr

#add below line above it
ExecStart=/root/vgpu_unlock/vgpu_unlock /bin/nvidia-vgpu-mgr


nano /usr/lib/nvidia/systemd/nvidia-vgpu-mgr.service

#comment below line
#ExecStart=/usr/bin/nvidia-vgpu-mgr

#add below line above it
ExecStart=/root/vgpu_unlock/vgpu_unlock /bin/nvidia-vgpu-mgr


systemctl daemon-reload




-------------------------------------------------------------------------


#nano /usr/src/nvidia-450.80/nvidia/os-interface.c
nano /usr/src/nvidia-460.73.01/nvidia/os-interface.c


# below ---- #include "nv-time.h"
# add this line

#include "/root/vgpu_unlock/vgpu_unlock_hooks.c"



nano /usr/src/nvidia-510.47.03/nvidia

nano /usr/src/nvidia-450.80/nvidia/nvidia.kbuild

nano /usr/src/nvidia-460.73.01/nvidia/nvidia.Kbuild


#below line #NV_CONFTEST_GENERIC

add

ldflags-y += -T /root/vgpu_unlock/kern.ld





--------------------------

dkms remove -m nvidia -v 450.80 --all

dkms install -m nvidia -v 450.80


reboot



dmseg

nvidi-smi


mdevctl types



-------------------VGPU_Profile------------------------------
mdevctl types

0000:01:00.0
   nvidia-531
    Available instances: 3
    Device API: vfio-pci
    Name: NVIDIA RTXA6000-16Q
    Description: num_heads=4, frl_config=60, framebuffer=16384M, max_resolution=7680x4320, max_instance=3



uuidgenerator.net

996f679e-e324-11ec-8ba6-3b82d5d8581d
996f6852-e324-11ec-8ba7-2f17487dddca
996f687a-e324-11ec-8ba8-df7805bd567d
996f68a2-e324-11ec-8ba9-cfe187ac5a0a




pci express bus location
mdevctl start -u 4e33d9a3-a5e4-4d1f-825f-9db06010ee44 -p 0000:42.00.0 --type nvidia-259



0000:01:00.0 Off

mdevctl start -u 996f679e-e324-11ec-8ba6-3b82d5d8581d -p 0000:01:00.0 -t nvidia-47
mdevctl start -u 996f6852-e324-11ec-8ba7-2f17487dddca -p 0000:01:00.0 -t nvidia-47
mdevctl start -u 996f687a-e324-11ec-8ba8-df7805bd567d -p 0000:01:00.0 -t nvidia-47
mdevctl start -u 996f68a2-e324-11ec-8ba9-cfe187ac5a0a -p 0000:01:00.0 -t nvidia-47


mdevctl define -a -u 996f679e-e324-11ec-8ba6-3b82d5d8581d
mdevctl define -a -u 996f6852-e324-11ec-8ba7-2f17487dddca
mdevctl define -a -u 996f687a-e324-11ec-8ba8-df7805bd567d
mdevctl define -a -u 996f68a2-e324-11ec-8ba9-cfe187ac5a0a


# mdevctl define -a -u f358fd7d-fdcd-454d-89d9-32d78968a818



--------------------


nano /etc/pve/qemu-server/100.conf

args: -uuid 00000000-0000-0000-0000-000000000100


args: -device 'vfio-pci,sysfsdev=/sys/bus/mdev/devices/996f679e-e324-11ec-8ba6-3b82d5d8581d' -uuid 00000000-0000-0000-0000-000000000100


args: -uuid 79a21fc8-41d5-4d31-9532-3013e71962f7100


args: -device 'vfio-pci,sysfsdev=/sys/bus/mdev/devices/79a21fc8-41d5-4d31-9532-3013e71962f7,display=off,id=hostpci0.0,bus=ich9-pcie-port-1,addr=0x0.0,x-pci-vendor-id=0x10de,x-pci-device-id=0x1c31' -uuid 00000000-0000-0000-0000-000000000100



args: -device 'vfio-pci,sysfsdev=/sys/bus/mdev/devices/33fac024-b107-11ec-828d-23fa8d25415b,display=off,id=hostpci0.0,bus=ich9-pcie-port-1,addr=0x0.0,x-pci-vendor-id=0x10de,x-pci-device-id=0x1c31' -uuid 00000000-0000-0000-0000-000000000100


args: -device 'vfio-pci,sysfsdev=/sys/bus/mdev/devices/79a21fc8-41d5-4d31-9532-3013e71962f7,display=off,id=hostpci0.0,bus=ich9-pcie-port-1,addr=0x0.0,x-pci-vendor-id=0x10de,x-pci-device-id=0x1e30,x-pci-sub-vendor-id=0x10de,x-pci-sub-device-id=0x12ba' -uuid 00000000-0000-0000-0000-000000000100













https://wvthoog.nl/proxmox-7-vgpu-v2/


------------------rust-----------------------
sudo apt update && sudo apt upgrade




curl https://sh.rustup.rs -sSf | sh


source $HOME/.cargo/env

------












<graphics type="spice" autoport="yes">
  <listen type="address"/>
  <image compression="off"/>
</graphics>











































































```
dmesg | grep -i -e DMAR -e IOMMU
lsmod | grep nvidia
systemctl status nvidia-vgpu-mgr


nvidia-smi
lspci -s 02:00.0 -k


output of <mdevctl types>  == 0000:02:00.0

sudo mdevctl define --parent 0000:02:00.0 --type nvidia-257
# ffb1de0a-8958-459c-bac4-b55685aee509

sudo mdevctl define --parent 0000:02:00.0 --type nvidia-257
# 78cc2d61-d470-4964-bc23-6de0cfd3ae0f


sudo mdevctl start--parent 0000:02:00.0 --uuid ffb1de0a-8958-459c-bac4-b55685aee509 -type nvidia-257
sudo mdevctl start--parent 0000:02:00.0 --uuid 78cc2d61-d470-4964-bc23-6de0cfd3ae0f -type nvidia-257


sudo mdevctl list -d


sudo nvidia-smi vgpu -q



# Assign by libvirt

virsh edit <VM-NAME>


<hostdev mode='subsystem' type='mdev' managed='no' model='vfio-pci' display='on'>
  <source>
    <address uuid='78cc2d61-d470-4964-bc23-6de0cfd3ae0f'/>
  </source>
</hostdev>

----

<hostdev mode='subsystem' type='mdev' managed='no' model='vfio-pci' display='on'>
  <source>
    <address uuid='ffb1de0a-8958-459c-bac4-b55685aee509'/>
  </source>
  <address type='pci' domain='0x0000' bus='0x00' slot='0x05' function='0x0'/>
  <rom enabled='no'/>
</hostdev>

nano /etc/libvirt/qemu/win10.xml
```
