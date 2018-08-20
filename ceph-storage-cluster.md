Short version for:

http://docs.ceph.com/docs/master/start/quick-ceph-deploy/

admin node:

```
mkdir my-cluster
cd my-cluster
ceph-deploy new node1 node2
ceph-deploy install node1 node2
ceph-deploy mon create-initial
ls -l
	ceph.client.admin.keyring
	ceph.bootstrap-mgr.keyring
	ceph.bootstrap-osd.keyring
	ceph.bootstrap-mds.keyring
	ceph.bootstrap-rgw.keyring
	ceph.bootstrap-rbd.keyring
ceph-deploy admin node1 node2
ceph-deploy mgr create node1
ceph-deploy osd create --data /dev/sdc node1
ceph-deploy osd create --data /dev/sdc node2
ssh node1 sudo ceph health
	stdout HEALTH_OK

ceph-deploy mds create node1
ceph-deploy mon add node2
ssh node1 sudo ceph quorum_status

ceph-deploy mgr create node2
ssh node1 sudo ceph -s
```


clean_up:

```
cd my-cluster
ceph-deploy purge node1 node2
ceph-deploy purgedata node1 node2
ceph-deploy forgetkeys
rm ceph.*
cd ..
rm -rf .cephdeploy.conf
rm -rf .cache
```
