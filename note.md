## add 10.0.2.2 to localhost
packer will use 10.0.2.2 as host ip address, so set it first.

`sudo ifconfig lo:1 10.0.2.2 up`

Restart network service:

`sudo /etc/init.d/networking restart`

Restart dns service:

`sudo systemctl restart systemd-resolved.service`

## Debian install packer
><https://learn.hashicorp.com/tutorials/packer/getting-started-install>
><https://github.com/vagrant-libvirt/vagrant-libvirt>
```bash
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
echo "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/mirror.list.d/packer.list
sudo apt-get update && sudo apt-get install packer vagrant
vagrant plugin install vagrant-libvirt
```
> $(lsb_release -cs) can be 'buster'

## Customize SSH keys

To generate the ssh keys for provisionning and put it in the sshkeys/ folder :

    ssh-keygen -q -t rsa -N '' -C 'packer-kvm-default-key' -f sshkeys/id_rsa

To get the default ssh private key :

    curl https://raw.githubusercontent.com/goffinet/packer-kvm/master/sshkeys/id_rsa

To get the default ssh public key :

    curl https://raw.githubusercontent.com/goffinet/packer-kvm/master/sshkeys/id_rsa.pub

## packer 常用命令

- packer -h
- packer -validate debian.json
- packer build debian.json
- PACKER_LOG=1 PACKER_LOG_PATH=packer.log packer build --debug debian.json

## vagrant 常用命令

- vagrant box add <boxname> <boxpath>
- vagrant init <boxname>

- modify vagrantfile

```bash
    Vagrant.configure('2') do |config|
        config.vm.box = "precise64"
        config.vm.box_url = "http://files.vagrantup.com/precise64.box"
        config.vm.hostname = "buzbar"
        config.vm.define "foohost"
        config.vm.provider :libvirt do |vb|
            vb.name = "barhost"
        end
    end
```
- vagrant up
