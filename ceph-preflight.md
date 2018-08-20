Short version for:

http://docs.ceph.com/docs/master/start/quick-start-preflight/

admin host:

```
wget -q -O- 'https://download.ceph.com/keys/release.asc' | sudo apt-key add -
echo deb https://download.ceph.com/debian-luminous/ $(lsb_release -sc) main | sudo tee /etc/apt/sources.list.d/ceph.list
sudo apt update
sudo apt install -y ceph-deploy ceph-common
ssh-keygen
sudo vim /etc/hosts
```

all node host:

```
sudo apt install -y ntp python
sudo update-alternatives --config editor
sudo visudo
	Defaults:ceph !requiretty
Add public key from admin node
```