vid=0
hid=1
onevm migrate --live 0 1

网络context设置
- NETWORK_ADDRESS
– NETWORK_MASK
– GATEWAY
– GATEWAY6
– DNS

$ONE_SRC_CODE_PATH/share/scripts/vmcontext.sh /etc/init.d 
ln /etc/init.d/vmcontext.sh /etc/rc2.d/S01vmcontext.sh

CONTEXT = [
hostname = "MAINHOST",
ip_private = "$NIC[IP, NETWORK=\"public net\"]",
dns = "$NETWORK[DNS, NETWORK_ID=0]",
root_pass = "$IMAGE[ROOT_PASS, IMAGE_ID=3]",
ip_gen = "10.0.0.$VMID",
files_ds = "$FILE[IMAGE=\"certificate\"] $FILE[IMAGE=\"server_license\"]"
]

Context information is passed to the Virtual Machine via an ISO mounted as a partition. This information can be
defined in the VM template in the optional section called Context, with the following attributes:
上下文信息是通过ISO安装为一个分区传递给虚拟机。此信息可
在可选的部分称为上下文在VM模板定义的，具有以下属性：


Windows
Provisioning a Windows VM is performed the standard way in OpenNebula:
1. Register the Installation media (typically a DVD) into a Datastore
2. Create an empty datablock with an appropriate size, at least 10GB. Change the type to OS. If you are using a
qcow2 image, don’t forget to add DRIVER=qcow2 and FORMAT=qcow2.

3. Create a template that boots from CDROM, enables VNC, and references the Installation media and the Image
created in step 2.
4. Follow the typical installation procedure over VNC.
5. Perform a deferred disk-snapshot of the OS disk, which will be saved upon shutdown.
6. Shutdown the VM.



CONTEXT=[
NETWORK="YES",
SSH_PUBLIC_KEY="$USER[SSH_PUBLIC_KEY]",
USER_DATA="#cloud-config
bootcmd:
- ifdown -a
runcmd:
- curl http://10.0.1.1:8999/I_am_alive
write_files:
- encoding: b64
content: RG9lcyBpdCB3b3JrPwo=
owner: root:root
path: /etc/test_file
permissions: ’0644’
packages:
- ruby2.0" ]


onemarket list --server http://marketplace.c12g.com
onemarket show 4fc76a938fb81d3517000004 --server http://marketplace.c12g.com
cat marketplace_image.one

dd if=/dev/zero of=ubuntu.img bs=1 count=1 seek=8G

name = "ubuntu"
memory = 256
disk = [’file:PATH/ubuntu.img,xvda,w’]
vif = [’bridge=BRIDGE’]
kernel = "PATH/vmlinuz"
ramdisk = "PATH/initrd.gz"

sudo xm create ubuntu.xen
sudo xm console ubuntu
sudo xm shutdown ubuntu


# Confgiuration attributes (dummy driver)
NAME = "Private Network"
DESCRIPTION = "A private network for VM inter-communication"
BRIDGE = "bond-br0"
# Context attributes
NETWORK_ADDRESS = "10.0.0.0"
NETWORK_MASK = "255.255.255.0"
DNS = "10.0.0.1"
GATEWAY = "10.0.0.1"
#Address Ranges, only these addresses will be assigned to the VMs
AR=[TYPE = "IP4", IP = "10.0.0.10", SIZE = "100" ]
AR=[TYPE = "IP4", IP = "10.0.0.200", SIZE = "10" ]
# Confgiuration attributes (OpenvSwtich driver)
NAME = "Public"
DESCRIPTION = "Network with public IPs"
BRIDGE = "br1"
VLAN = "YES"
VLAN_ID = 12
DNS = "8.8.8.8"
GATEWAY = "130.56.23.1"
LOAD_BALANCER = 130.56.23.2
AR=[ TYPE = "IP4", IP = "130.56.23.2", SIZE = "1"]
AR=[ TYPE = "IP4", IP = "130.56.23.34", SIZE = "1"]
AR=[ TYPE = "IP4", IP = "130.56.23.24", SIZE = "1"]
AR=[ TYPE = "IP4", IP = "130.56.23.17", MAC= "50:20:20:20:20:21", SIZE = "1"]
AR=[ TYPE = "IP4", IP = "130.56.23.12", SIZE = "1"]

#注册file
oneimage create  -d 2 --name context --type CONTEXT --path /opt/context.sh