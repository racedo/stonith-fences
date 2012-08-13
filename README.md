stonith-fences
==============

Pacemaker stonith fencing agents for vmware fusion.

Installation instructions
cp fence* /usr/sbin on all servers running stonith.


pacemaker configuration example

primitive fusion-fencing stonith:fence_fusion \
    params ipaddr="MAC_IP_REACHABLE_FROM_CLUSTER_HOSTS" login="MAC_USERNAME" passwd="MAC_PASSWORD" \
    pcmk_host_list="box1.example.loc box2.example.loc" \
    pcmk_host_map="box1.example.loc:00_0C_29_93_11_B5;box2.example.loc:00_0C_29_6E_5A_43" \
    meta target-role="started"
property stonith-enabled="true"


** Note
   00_0C_29_6E_5A_43 is the mac address of any of the interfaces on one of the
vms that will be killed via stonith.  We are just replacing : with _  in the
syntax to fit into the stonith rules.

** Testing.
   kill -9 the corosync process on one of the cluster nodes
