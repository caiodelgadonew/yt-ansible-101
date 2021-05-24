# -*- mode: ruby -*-
# vi: set ft=ruby  :

machines = {
  "ansible"   => {"memory" => "1024", "cpu" => "2", "ip" => "30", "image" => "ubuntu/focal64"},
  "machine01" => {"memory" => "512",  "cpu" => "2", "ip" => "31", "image" => "debian/buster64"},
  "machine02" => {"memory" => "512",  "cpu" => "2", "ip" => "32", "image" => "centos/7"}
}

Vagrant.configure("2") do |config|

  machines.each do |name, conf|
    config.vm.define "#{name}" do |machine|
      machine.vm.box = "#{conf["image"]}"
      machine.vm.hostname = "#{name}.caiodelgado.example"
      machine.vm.network "private_network", ip: "10.10.10.#{conf["ip"]}"
      machine.vm.provider "virtualbox" do |vb|
        vb.name = "#{name}"
        vb.memory = conf["memory"]
        vb.cpus = conf["cpu"]
        vb.customize ["modifyvm", :id, "--groups", "/iac"]
      end
    config.vm.provision "shell", inline: <<-EOF
      HOSTS=$(head -n7 /etc/hosts)
      echo -e "$HOSTS" > /etc/hosts
      echo '10.10.10.30 ansible.caiodelgado.example' >> /etc/hosts
      echo '10.10.10.31 machine01.caiodelgado.example' >> /etc/hosts
      echo '10.10.10.32 machine02.caiodelgado.example' >> /etc/hosts
      EOF
    end
  end
end
