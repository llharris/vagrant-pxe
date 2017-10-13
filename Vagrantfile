# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # SSH Keypair Settings
  config.ssh.insert_key = false
  config.ssh.private_key_path = ["E:\\Home\\Projects\\Vagrant\\keys\\private", "~/.vagrant.d/insecure_private_key"]
  # Disable Synced Directory
  config.vm.synced_folder '.', '/vagrant', disabled: true
    # Provider-specific configuration.
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "512"
	vb.cpus = 1
  end
  # VM Specific Configuration
  config.vm.define "pxe" do |pxe|
    pxe.vm.box = "centos/7"
	pxe.vm.hostname = "pxe"
    pxe.vm.network "public_network", ip: "192.168.1.253"
    pxe.vm.provision "file", source: "E:\\Home\\Projects\\Vagrant\\keys\\public", destination: "~/.ssh/authorized_keys"
	pxe.vm.provision "shell", inline: <<-EOC
	  localectl set-keymap uk
	  sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
	  systemctl restart sshd
	  yum install screen mlocate zip unzip httpd epel-release bind-utils net-tools git policycoreutils-python libsemanage-python syslinux-tftpboot tftp tftp-server tcpdump wget -y
	  grep -vE "}|disable" /etc/xinetd.d/tftp > /etc/xinetd.d/tftp.new
	  echo -e "\tdisable\t\t\t= no" >> /etc/xinetd.d/tftp.new
	  echo -e "}" >> /etc/xinetd.d/tftp.new
	  mv -f /etc/xinetd.d/tftp.new /etc/xinetd.d/tftp
	  systemctl restart tftp
	  cp -ur /usr/share/syslinux/* /var/lib/tftpboot
	  mkdir /var/lib/tftpboot/pxelinux.cfg
	  mkdir /var/lib/tftpboot/centos7
	  touch /var/lib/tftpboot/pxelinux.cfg/default
	  wget -q -O /var/lib/tftpboot/pxelinux.cfg/default https://raw.githubusercontent.com/llharris/vagrant-pxe/master/default
	  wget -q -O /var/lib/tftpboot/centos7/initrd.img http://mirror.centos.org/centos/7/os/x86_64/images/pxeboot/initrd.img
	  wget -q -O /var/lib/tftpboot/centos7/vmlinuz http://mirror.centos.org/centos/7/os/x86_64/images/pxeboot/vmlinuz
	EOC
  end	
end
