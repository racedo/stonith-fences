# Fence Agent for VMware Fusion

 Pacemaker stonith fencing agents for VMware Fusion.

## Installation instructions
`cp fence* /usr/sbin` on all servers running stonith.

## Pacemaker configuration example

```bash
pcs stonith create fence_with_fusion fence_fusion \
ipaddr=192.168.100.1 \
pcmk_host_map="controller1.vm.lab:00_0C_29_94_A2_36;controller2.vm.lab:00_0C_29_87_5F_A3;controller3.vm.lab:00_0C_29_81_19_CD" \
pcmk_host_list="controller1.vm.lab controller2.vm.lab controller3.vm.lab" \
login=racedo \
identity-file=/root/.ssh/id_rsa \
meta target-role="started" --force
property stonith-enabled="true"
```

**Note**
   The MAC addresses are of any of the interfaces on one of the
VMs that will be killed via stonith.  We are just replacing : with _ in the
syntax to fit into the stonith rules.

## Testing
#### Manually testing the fence agent with a preconfigured ssh-pair with the host and the VMs

   `python  fence_fusion --ssh -a 192.168.100.1 --ssh -l racedo --identity-file /root/.ssh/id_rsa -n 00_0c_29_94_a2_36 --login-timeout=10`

#### Testing directly with Pacemaker
   On one of the hosts run: `killall -9 corosync`
