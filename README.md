# Fence Agent for VMware Fusion

 This Pacemaker stonith fence agent is a modified version of Eric Edgar's original fence agent by Ramon Acedo and has been tested on VMware Fusion 6.0.5 on Mac OS X 10.10.1 and RHEL 7 with Pacemaker 1.1.10

## Installation instructions
`cp fence_fusion /usr/sbin` on all servers running stonith.

## Pacemaker configuration example

```bash
pcs stonith create fence_with_fusion fence_fusion \
ipaddr=10.0.0.1 \
pcmk_host_map="pcmk-controller1:00_0c_29_ee_ed_78;pcmk-controller2:00_0c_29_7a_97_f2" \
pcmk_host_list="pcmk-controller1 pcmk-controller2" \
login=racedo \
identity_file=/root/.ssh/id_rsa \
secure=1 \
meta target-role="started" --force
```

`pcs property set stonith-enabled="true"`

**Note**
The MAC addresses are from any of the NICs on the VMs that will be killed via stonith.  We are just replacing : with _ in the syntax to fit into the stonith rules.

## Testing
#### Manually testing the fence agent with a preconfigured ssh-pair with the host and the VMs

   `python /usr/sbin/fence_fusion --ip 192.168.100.1 --ssh -l racedo --identity-file /root/.ssh/id_rsa --plug 00_0C_29_94_A2_36 --login-timeout=10 --action reboot`

#### Testing directly with Pacemaker
   On one of the hosts of the cluster run either of these commands: 

   `stonith_admin --reboot pcmk-controller1`

   `pcs stonith fence  pcmk-controller1`

   `killall -9 corosync`

