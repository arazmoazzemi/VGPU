# Ubuntu VGP Unlock

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
