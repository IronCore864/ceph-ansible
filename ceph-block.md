Short version for:

http://docs.ceph.com/docs/master/start/quick-rbd/
admin:

```
ceph-deploy install client
ceph-deploy admin client
ssh node1 sudo ceph osd pool create rbd 8
ssh node1 sudo rbd pool init rbd
```

client

```
sudo rbd create foo --size 4096 --image-feature layering
sudo rbd map foo --name client.admin
sudo mkfs.ext4 -m0 /dev/rbd/rbd/foo
sudo mkdir /mnt/ceph-block-device
sudo mount /dev/rbd/rbd/foo /mnt/ceph-block-device
cd /mnt/ceph-block-device
```