Vagrant.configure("2") do |config|
    MasterCount = 1
    (1..MasterCount).each do |i|
        config.vm.define "k8s-master-#{i}" do |master|
            master.vm.box = "generic/ubuntu2004"
            master.vm.box_version = "3.6.8"
            master.vm.hostname = "k8s-master-#{i}"
	        master.vm.network "public_network", ip: "172.20.33.10#{i}"
            master.vm.provider "virtualbox" do |vb|
                vb.name = "k8s-master-#{i}"
                vb.memory = 2048
                vb.cpus = 2
            end
            master.vm.provision "shell" do |s|
                s.inline = <<-SHELL
                    echo 172.20.33.10#{i} k8s-master-#{i} >> /etc/hosts
                SHELL
            end
        end
    end

    WorkerCount = 2
    (1..WorkerCount).each do |i|
        config.vm.define "k8s-worker-#{i}" do |worker|
            worker.vm.box = "generic/ubuntu2004"
            worker.vm.box_version = "3.6.8"
            worker.vm.hostname = "k8s-worker-#{i}"
            worker.vm.network "public_network", ip: "172.20.33.11#{i}"
            worker.vm.provider "virtualbox" do |vb|
                vb.name = "k8s-worker-#{i}"
                vb.memory = 3060
                vb.cpus = 2
            end
            worker.vm.provision "shell" do |s|
                s.inline = <<-SHELL
                    echo 172.20.33.11#{i} k8s-worker-#{i} >> /etc/hosts
                SHELL
            end
        end
    end

    config.vm.provision "shell" do |s|
        ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
        s.inline = <<-SHELL
            # Create ci user
            useradd -s /bin/bash -d /home/ci/ -m -G sudo ci
            echo 'ci ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers
            mkdir -p /home/ci/.ssh && chown -R ci /home/ci/.ssh
            echo #{ssh_pub_key} >> /home/ci/.ssh/authorized_keys
        SHELL
    end

end
