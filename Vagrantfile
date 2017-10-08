PUBLIC_IP = "192.168.178.51"

Vagrant.configure("2") do |config|
   config.vm.box = "centos/7"
  
   config.vm.network "public_network", ip: PUBLIC_IP
   
   config.vm.synced_folder '.', '/vagrant', disabled: true
 
   config.vm.provider "virtualbox" do |vb|
     # Display the VirtualBox GUI when booting the machine
     vb.gui = false
  
     # Customize the amount of memory on the VM:
     vb.memory = "4096"
	 vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
   end
   
   config.vm.provision "file", run: "always", source: "daemon.json", destination: "/tmp/daemon.json"

   config.vm.provision "shell" do |s|
     s.inline = "sudo yum -y update && sudo yum -y install centos-release-openshift-origin36.noarch && sudo yum -y install origin-clients.x86_64 && sudo yum -y install docker.x86_64"
   end
   
   config.vm.provision "shell", run: "always" do |s|
     s.inline = "sudo cp /tmp/daemon.json /etc/docker/daemon.json"
   end
   
   config.vm.provision "shell", run: "always" do |s|
     s.inline = "sudo service docker restart"
   end

   config.vm.provision "shell", run: "always" do |s|
     s.inline = "sudo oc cluster up --public-hostname="+PUBLIC_IP+" --host-config-dir='/var/lib/origin/openshift.local.config' --host-pv-dir='/var/lib/origin/openshift.local.pv' --host-volumes-dir='/var/lib/origin/openshift.local.volumes' --host-data-dir='/var/lib/origin/openshift.local.data'"
   end
   
#   config.vm.provision "shell", run: "always" do |s|
#     s.inline = "sudo setenforce 0"
#   end
   
#   config.vm.provision "shell", run: "always" do |s|
#     s.inline = "sudo docker run -i --rm --privileged -v /var/run/docker.sock:/var/run/docker.sock -v /var/lib/che:/data -e CHE_HOST="+PUBLIC_IP+" eclipse/che start"
#   end
   
   
end
