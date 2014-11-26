# Fence Agent for VMware Fusion

 This Pacemaker stonith fence agent is a modified version of Eric Edgar's original fence agent by Ramon Acedo and has been tested on VMware Fusion 6.0.5 on Mac OS X 10.10.1 and RHEL 7 with Pacemaker 1.1.10

## Installation instructions
`cp fence_fusion /usr/sbin` on all servers running stonith.

## Pacemaker configuration example

```bash
pcs stonith create fence_with_fusion fence_fusion \
ipaddr=192.168.100.1 \
pcmk_host_map="controller1.vm.lab:00_0C_29_94_A2_36;controller2.vm.lab:00_0C_29_87_5F_A3;controller3.vm.lab:00_0C_29_81_19_CD" \
pcmk_host_list="controller1.vm.lab controller2.vm.lab controller3.vm.lab" \
login=racedo \
identity-file=/root/.ssh/id_rsa \
meta target-role="started" --force
```

`pcs property stonith-enabled="true"`

**Note**
The MAC addresses are from any of the NICs on the VMs that will be killed via stonith.  We are just replacing : with _ in the syntax to fit into the stonith rules.

## Testing
#### Manually testing the fence agent with a preconfigured ssh-pair with the host and the VMs

   `python /usr/sbin/fence_fusion --ssh --ip 192.168.100.1 --ssh -l racedo --identity-file /root/.ssh/id_rsa --plug 00_0C_29_94_A2_36 --login-timeout=10 --action reboot`

#### Testing directly with Pacemaker
   On one of the hosts of the cluster run either of these commands: 

   `stonith_admin --reboot controller1.vm.lab`

   `pcs stonith fence  controller1.vm.lab`

   `killall -9 corosync`

