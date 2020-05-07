# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_NO_PARALLEL'] = 'yes'
Vagrant.configure(2) do |config| # Vagrant konfigürasyonlarının yapılması için kullanılan komut ana fonksiyon 
  config.vm.provision "shell", path: "bootstrap.sh" # Bu komut shell de bootstrap.sh adında bir dosyanın çalıştırılması için kullanılıyor.
  # Kubernetes Master Server
  config.vm.define "kodiksmaster" do |kmaster| # kodiksmaster makinesinin tanımlarının yapılması için kullanılan ana fonksiyon
    kmaster.vm.box = "centos/7" # kodiksmaster makinesine kurulacak işletim sistemi
    kmaster.vm.hostname = "kodiksmaster.example.com" #kodiksmakine adı 
    kmaster.vm.network "private_network", ip: "192.168.20.100" #network seçimi private_network host_only network anlamına geliyor
    kmaster.vm.provider "virtualbox" do |v| #sanal makine uygulaması kullanılacağını seçiyoruz(virtualbox) mesela vmware de olabilirdi 
      v.name = "kodiksmaster" #sanal makine adı
      v.memory = 2048 # ram
      v.cpus = 2 # cpu
    end
    kmaster.vm.provision "shell", path: "bootstrap_kmaster.sh"
  end
  NodeCount = 2
  # Kubernetes Worker Nodes
  (1..NodeCount).each do |i|
    config.vm.define "kodiksworker#{i}" do |workernode|
      workernode.vm.box = "centos/7"
      workernode.vm.hostname = "kodiksworker#{i}.example.com"
      workernode.vm.network "private_network", ip: "192.168.20.10#{i}"
      workernode.vm.provider "virtualbox" do |v|
        v.name = "kodiksworker#{i}"
        v.memory = 1024
        v.cpus = 1
      end
      workernode.vm.provision "shell", path: "bootstrap_kworker.sh"
    end
  end
end
