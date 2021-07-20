# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.box = "mmichal/ubuntu16_04"
  config.vm.box_version = "1.1.20210331"
  config.vm.network "forwarded_port", guest: 80, host: 80, host_ip: "127.0.0.1"
  config.vm.network "forwarded_port", guest: 443, host: 443, host_ip: "127.0.0.1"
  config.vm.network "private_network", ip: "192.168.123.123"

   config.vm.provider "virtualbox" do |vb|
     vb.gui = false
     vb.memory = "4096"
    end
####################For Minikube Kubernetes####################
config.vm.provision "shell", path: "scripts/install-minikube.sh"
####################Apply configuration####################
config.vm.provision "shell", inline: <<-SHELL
echo "Add&Update helm repo"
sudo helm repo add grafana https://grafana.github.io/helm-charts
sudo helm repo update

echo "Create namespace"
sudo kubectl create ns loki

echo "Install Loki stack"
sudo helm upgrade --install loki grafana/loki-stack  --set grafana.enabled=true,prometheus.enabled=true,prometheus.alertmanager.persistentVolume.enabled=false,prometheus.server.persistentVolume.enabled=false,plugins=grafana-piechart-panel -n loki
sudo kubectl apply -f /vagrant/files/grafana-ingress.yaml
SHELL
end