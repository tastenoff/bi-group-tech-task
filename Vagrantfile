Vagrant.configure("2") do |config|
  # Устанавливаем глобальный конфиг
  config.vm.box = "generic/oracle8"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = 2
  end

  # VM-1: docker01
  config.vm.define "docker01" do |docker01|
    docker01.vm.hostname = "docker01"
    docker01.vm.network "forwarded_port", guest: 9090, host: 9090, auto_correct: true
    docker01.vm.network "forwarded_port", guest: 3000, host: 3000, auto_correct: true
  end

  # VM-2: etcd01
  config.vm.define "etcd01" do |etcd01|
    etcd01.vm.hostname = "etcd01"
  end

  # VM-3: pgsql01
  config.vm.define "pgsql01" do |pgsql01|
    pgsql01.vm.hostname = "pgsql01"
    pgsql01.vm.network "forwarded_port", guest: 5432, host: 5432, auto_correct: true
  end

  # VM-4: pgsql02
  config.vm.define "pgsql02" do |pgsql02|
    pgsql02.vm.hostname = "pgsql02"
    pgsql02.vm.network "forwarded_port", guest: 5432, host: 5433, auto_correct: true
  end
end

