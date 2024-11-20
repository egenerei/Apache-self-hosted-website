Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"
  config.ssh.insert_key = false
  config.vm.hostname = "web1" # Set the hostname

  config.vm.provider "virtualbox" do |vb|
    # vb.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
    # vb.customize ["modifyvm", :id, "--natdnsproxy1", "off"]
    vb.memory = "1024"
    vb.cpus = 12  
  end

  config.vm.provision "shell", inline: <<-SCRIPT
    timedatectl set-timezone Europe/Madrid
    apt update
    apt install prometheus -y
    apt install prometheus-apache-exporter -y
    cp /vagrant/prometheus.yml /etc/prometheus/
    apt-get install -y apt-transport-https software-properties-common wget
    mkdir -p /etc/apt/keyrings/
    wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
    echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
    apt-get update
    apt-get install grafana -y
    systemctl start grafana-server
    systemctl enable grafana-server
  SCRIPT

  config.vm.define "web_non_production" do |web1|
    web1.vm.network "public_network"
  end

end
# , ip: "192.168.1.40", bridge: "enp6s0"
# ip route replace default via 192.168.1.1 dev eth1 i