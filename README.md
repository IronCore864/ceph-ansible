# What It Is

Installs a Ceph cluster with ansible, including admin and client nodes.

# Dependencies

- Ansible
- Vagrant
- Virtualbox

# Deploy

```
git clone git@github.com:KI-labs/ceph-ansible.git
cd ceph-ansible
vagrant up
ansible-playbook -i inventories/local preflight.yml
ansible-playbook -i inventories/local storage_cluster.yml
ansible-playbook -i inventories/local client.yml
```

Notes:

1. The `vagrant up` will create 4 VMs so it's slow, be patient, and there is a chance that your laptop would crush depending on the hardware (8G mem here and I crashed twice) :)

2. The first playbook "preflight.yml" installs ceph-deploy, manages dependency packages, and prepare SSH cert

3. The second playbook "storage_cluster.yml" creates the storage cluster and installs two monitors and two managers

4. The last playbook "client.yml" creates an rbd and mount it, as mentioned in the next section of this doc

# Local ENV

Also a local environment by vagrant/virtualbox is included. Detailed info:

- Ubuntu 18.04 LTS
- 1 admin node to do ceph-deploy, IP 10.2.2.2
- 2 node, 10G disk each, node1 IP: 10.2.2.3, node2 IP: 10.2.2.4
- 1 client node for testing, IP 10.2.2.5
- a rbd pool named "rbd" is initialized
- a 4096MB rbd named "foo" is created `/dev/rbd/rbd/foo` and mounted to `/mnt/ceph-block-device`

# Test

```
vagrant ssh client
cd /mnt/ceph-block-device
echo "Hello world!" > hello.txt
cat hello.txt
```

# Clean Cluster and Retry

If it fails in the storage cluster playbook and you want to retry, do the following:

```
vagrant ssh admin
cd my-cluster
ceph-deploy purge node1 node2
ceph-deploy purgedata node1 node2
ceph-deploy forgetkeys
rm ceph.*
cd ..
rm -rf .cephdeploy.conf
rm -rf .cache
exit
```

If OSDs are already created, you need to delete it in the nodes too:

```
vagrant ssh node1 [and node2 too]
vgdisplay
vgremove xxx
exit
```

If it fails at client playbook, you want to remove the rbd and umount the mounted disk:

```
vagrant ssh client
sudo rbd remove foo
sudo umount -l /mnt/ceph-block-device
exit
```

Then replay.

If this is too much information, just:

```
vagrant destroy
```

then start over again. It takes time so get a coffee in the meantime :)

# Default Values

Default values for the local ENV see `inventories/local/group_vars/*`.

# Ansible Structure

Follows https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html#alternative-directory-layout

Not sure if this is better than the first layout which is:
https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html#directory-layout

# Manual Setup

If you prefer manual setup but the official doc is too long for you, here are the short versions:

```
ceph-preflight.md
ceph-storage-cluster.md
ceph-block.md
```

# Todo

- Steps for check points are omitted but left in the comment, need to be implemented
- Refactor for better structure
- Add ansible vault for private key
- etc
