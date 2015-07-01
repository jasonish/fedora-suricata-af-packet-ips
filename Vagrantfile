# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "jasonish/fedora-cloud-22"
  config.vm.box_check_update = false

  # This will map to the second interface, likely eth1 as a VirtualBox
  # internal network.  Place another VM, your "protected" host on the
  # same internal network.
  config.vm.network "private_network", ip: "7.0.0.9",
                    virtualbox__intnet: true

  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = "1024"
    vb.cpus = 2

    vb.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
  end

  config.vm.provision "shell", inline: <<-SHELL
    dnf -y update
    dnf -y install git \
                   kernel-devel \
                   gcc \
                   gcc-c++ \
                   autoconf \
                   automake \
                   libtool \
                   libpcap-devel \
                   jansson-devel \
                   which \
                   pcre-devel \
                   libyaml-devel \
                   file-devel \
                   zlib-devel \
                   make \
                   ccache \
                   net-tools \
                   tcpdump

    test -e libhtp || git clone https://github.com/OISF/libhtp.git -b 0.5.x
    test -e suricata || git clone https://github.com/inliniac/suricata.git
    chown -R vagrant:vagrant .
    test -e /usr/bin/suricata || (cd suricata && \
      cp -a ../libhtp . && \
      ./autogen.sh && \
      ./configure --prefix=/usr/ --sysconfdir=/etc/ --localstatedir=/var/ && \
      make && \
      make install install-conf)
  SHELL
end
