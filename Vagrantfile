Vagrant.configure("2") do |config|
	config.vm.define :server do |server|
		server.vm.box = "bento/centos-7.9"
		server.vm.network :public_network, ip: "192.168.100.2"
		server.vm.hostname = "server"
		server.vm.provision "shell", inline:<<-SHELL
			yum update -y
			yum install -y curl wget
			yum install -y java-11-openjdk-devel
			sudo yum install -y java-1.8.0-openjdk
			mkdir data
			mkdir data/streama
			cd data
			wget https://github.com/streamaserver/streama/releases/download/v1.7.3/streama-1.7.3.jar
		SHELL
	end

	config.vm.define :firewall do |firewall|
		firewall.vm.box = "bento/centos-7.9"
		firewall.vm.network :public_network, ip: "192.168.100.3"
		firewall.vm.hostname = "firewall"
		firewall.vm.provision "shell", inline:<<-SHELL
			systemctl start firewalld
			firewall-cmd --zone=public --add-service=http --permanent
			firewall-cmd --zone=public --add-masquerade --permanent
			firewall-cmd --zone="public" --add-forward-port=port=80:proto=tcp:toport=8080:toaddr=192.168.100.2 --permanent
			firewall-cmd --zone="public" --add-forward-port=port=8080:proto=tcp:toport=8080:toaddr=192.168.100.2 --permanent
		SHELL
	end
end