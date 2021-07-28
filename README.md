# OpenVAS Setup on Ubuntu-18.04

## Table of Contents

  - [Install VirtualBox and dependencies](#install-virtualbox-and-dependencies)
  - [Install vagrant](#install-vagrant)
  - [Create Vagrantfile](#create-vagrantfile)
  - [Boot vagrant box and ssh into it](#boot-vagrant-box-and-ssh-into-it)
  - [Update Ubuntu](#update-ubuntu)
  - [Add OpenVAS repository to system](#add-openvas-repository-to-system)
  - [Install sqlite3 for database](#install-sqlite3-for-database)
  - [Enable pdf reports](#enable-pdf-reports)
  - [Install openvas-nasl utility](#install-openvas-nasl-utility)
  - [Download the Network Vulnerability](#download-the-network-vulnerability)
  - [Restart OpenVAS and OpenVAS Manager](#restart-openvas-and-openvas-manager)
  - [Validate OpenVAS service](#validate-openvas-service)
  - [Rebuild the OpenVAS database](#rebuild-the-openvas-database)
  - [Check the OpenVAS](#check-the-openvas)
  - [Open the dashboard in the web browser](#open-the-dashboard-in-the-web-browser)
  - [Login credentials](#login-credentials)
  - [Change the admin password](#change-the-admin-password)
  - [Private network](#private-network)

### Install VirtualBox and dependencies


```bash
sudo apt-get install -y virtualbox virtualbox-dkms virtualbox-guest-additions-iso virtualbox-guest-dkms virtualbox-guest-source curl wget;
```
### Install vagrant


```bash
mkdir -p ~/OpenVAS/vms && cd $_
curl -O https://releases.hashicorp.com/vagrant/2.2.18/vagrant_2.2.18_x86_64.deb;
sudo dpkg -i ./vagrant_2.2.18_x86_64.deb;
vagrant --version;
mv vagrant_2.2.18_x86_64.deb ../
```

### Create Vagrantfile


```bash
cat <<EOF >Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-18.04"
end
EOF
```

### Boot vagrant box and ssh into it


```bash
vagrant up;
vagrant ssh;
```

### Update Ubuntu


```bash
sudo apt-get update;
sudo apt-get upgrade;
```

### Add OpenVAS repository to system


```bash
sudo apt-get install software-properties-common;
sudo add-apt-repository ppa:mrazavi/openvas;
sudo apt-get install openvas9 -y; # Note: Press <Yes> when you get the pop window.
```

### Install sqlite3 for database


```bash
sudo apt-get install sqlite3;
```

### Enable pdf reports


```bash
sudo apt-get install -y texlive-latex-extra --no-install-recommends;
sudo apt-get install -y texlive-fonts-recommended;
```

### Install openvas-nasl utility


```bash
sudo apt-get install -y libopenvas9-dev;
```

### Download the Network Vulnerability


- Run the commands below to download the Network Vulnerability, tests from OpenVAS Feed and sync security content automation protocol data and cert vulnerability data.
-------------------------------------

```bash
sudo greenbone-nvt-sync;      # Please skip this command
sudo greenbone-scapdata-sync; # Please skip this command
sudo greenbone-certdata-sync; # Please skip this command
```

### Restart OpenVAS and OpenVAS Manager


```bash
sudo service openvas-scanner restart;
sudo service openvas-manager restart;
sudo service openvas-gsa restart;
```

### Validate OpenVAS service

```bash
sudo service openvas-scanner status;
```

### Rebuild the OpenVAS database

```bash
sudo openvasmd --rebuild --progress;
```

### Check the OpenVAS

```browser
curl --insecure https://localhost:4000
```

### Open the dashboard in the web browser


```browser
https://server.address.com:4000
```

### Login credentials

- un: admin
- pw: admin


### Change the admin password


```bash
sudo openvasmd --user=admin --new-password=admin@321
```

### Private network


Will setup private network in Vagrant it self to avoid LAN. This way we could setup infrastructure fast and easy.
