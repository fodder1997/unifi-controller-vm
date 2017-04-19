# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu-14.04"
  config.vm.box_download_checksum_type = "sha256"
  #config.vm.box_download_checksum = "b39ccb6013084d8487bb8240cab74c2fa10aa4a4a6134d10e87e006d61b9681d"
  config.vm.box_download_checksum = "8d5fd0a90d0cffd8a5ed84e608ee8665475efb7370da80c03653390f46de026b"
  #config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/20150924/trusty-server-cloudimg-amd64-vagrant-disk1.box"
  config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"

  # Use bridged network adapter for VM
  # http://docs.vagrantup.com/v2/networking/public_network.html
  config.vm.network "public_network", bridge: "eth0", ip: "192.168.2.110"


  config.vm.provider "virtualbox" do |v|
  	v.memory = 2048
  end
  
  # Map web interface to localhost - as a convenience
  # https://localhost:8443
  # config.vm.network :forwarded_port, host: 8443, guest: 8443

  config.vm.provision "shell", inline: <<-shell
    set -e;

    # Add support for future upgrades via apt
    sudo cat "deb http://www.ubnt.com/downloads/unifi/debian unifi5 ubiquiti" > /etc/apt/sources.list.d/100-ubnt.list
    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv C0A52C50
    sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com 06E85760C0A52C50

    echo "Unifi Controller v$UNIFI_VERSION is now listening at"

    apt-get update
    apt-get upgrade -y --force-yes
    apt-get install -y openjdk-7-jre-headless jsvc mongodb-server

    # 10gen
    #sudo apt-key adv --keyserver keyserver.ubuntu.com --recv 7F0CEB10
    #echo "deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen" >> /etc/apt/sources.list.d/10gen.list
    #apt-get update
    #apt-get install -f mongodb-10gen

    # http://www.ubnt.com/download/unifi/
    cd /home/vagrant
    UNIFI_VERSION="4.8.20"
    UNIFI_FILENAME="unifi_sysvinit_all.deb"
    wget https://www.ubnt.com/downloads/unifi/$UNIFI_VERSION/$UNIFI_FILENAME
    sha256sum -c /vagrant/sha256.txt && dpkg -i $UNIFI_FILENAME

    echo "Unifi Controller v$UNIFI_VERSION is now listening at"
    for i in `hostname --ip-address`
    do
      echo "https://$i:8443"
    done
  shell

end
