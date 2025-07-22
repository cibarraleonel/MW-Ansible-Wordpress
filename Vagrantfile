Vagrant.configure("2") do |config|

  boxes = {
    "ubuntu" => {
      box: "ubuntu/jammy64",
      ip: "192.168.56.101"
    },
    "debian" => {
      box: "debian/bookworm64",
      ip: "192.168.56.102"
    },
    "rocky" => {
      box: "generic/rocky9",
      ip: "192.168.56.103"
    }
  }

  boxes.each do |name, conf|
    config.vm.define name do |vm|
      vm.vm.box = conf[:box]
      vm.vm.hostname = "#{name}.local"
      vm.vm.network "private_network", ip: conf[:ip]

      vm.vm.provider "virtualbox" do |vb|
        vb.memory = 1024
        vb.cpus = 2
      end

    end
  end
end
