Vagrant.configure("2") do |config|
  # ---------------- IP-addresses and ports ----------------
  SERVER_IP         = ENV['SERVER_IP']         || "192.168.178.110" 
  SERVER2_IP        = ENV['SERVER2_IP']        || "192.168.178.120"  
  DATABASE_IP       = ENV['DATABASE_IP']       || "192.168.178.111"
  LOAD_BALANCER_IP  = ENV['LOAD_BALANCER_IP']  || "192.168.178.112"
  JENKINS_IP        = ENV['JENKINS_IP']        || "192.168.178.113"

  SERVER_PORT        = (ENV['SERVER_PORT']        || 8080).to_i
  DATABASE_PORT      = (ENV['DATABASE_PORT']      || 5432).to_i
  LOAD_BALANCER_PORT = (ENV['LOAD_BALANCER_PORT'] || 80).to_i
  BRIDGE_IF          = ENV['BRIDGE']              || "Automatic"

  # ---------------- SERVER (Flask) ----------------
  config.vm.define "server" do |server|
    server.vm.box = "bento/ubuntu-24.04"
    server.vm.network "private_network", ip: SERVER_IP
    # server.vm.network "forwarded_port", guest: 80, host: SERVER_PORT

    server.vm.synced_folder "./shared-images", "/opt/flaskapp/static/images",
      owner: "vagrant",
      group: "vagrant",
      mount_options: ["dmode=775", "fmode=664"]

    server.vm.provider "vmware_desktop" do |v|
      v.vmx["displayName"] = "Flask Server 1"
      v.vmx["memsize"]     = "2048"
      v.vmx["numvcpus"]    = "2"
    end
  end

  # ---------------- SERVER2 (Flask) ----------------
  config.vm.define "server2" do |server2|
    server2.vm.box = "bento/ubuntu-24.04"
    server2.vm.network "private_network", ip: SERVER2_IP
    # server2.vm.network "forwarded_port", guest: 80, host: SERVER_PORT + 1

    server2.vm.synced_folder "./shared-images", "/opt/flaskapp/static/images",
      owner: "vagrant",
      group: "vagrant",
      mount_options: ["dmode=775", "fmode=664"]

    server2.vm.provider "vmware_desktop" do |v|
      v.vmx["displayName"] = "Flask Server 2"
      v.vmx["memsize"]     = "2048"
      v.vmx["numvcpus"]    = "2"
    end
  end

  # ---------------- DATABASE ----------------
  config.vm.define "db" do |db|
    db.vm.box = "bento/ubuntu-24.04"
    db.vm.network "private_network", ip: DATABASE_IP
    # db.vm.network "forwarded_port", guest: 5432, host: DATABASE_PORT

    db.vm.provider "vmware_desktop" do |v|
      v.vmx["displayName"] = "PostgreSQL Server"
      v.vmx["memsize"]     = "2048"
      v.vmx["numvcpus"]    = "2"
    end
  end

  # ---------------- LOAD BALANCER ----------------
  config.vm.define "load_balancer" do |lb|
    lb.vm.box = "bento/ubuntu-24.04"
    lb.vm.network "private_network", ip: LOAD_BALANCER_IP
    # lb.vm.network "forwarded_port", guest: 80, host: LOAD_BALANCER_PORT

    lb.vm.provider "vmware_desktop" do |v|
      v.vmx["displayName"] = "Load Balancer Server"
      v.vmx["memsize"]     = "2048"
      v.vmx["numvcpus"]    = "2"
    end
  end

  # ---------------- JENKINS ----------------
  config.vm.define "jenkins" do |jenkins|
    jenkins.vm.box = "bento/ubuntu-24.04"
    jenkins.vm.network "private_network", ip: JENKINS_IP
    # jenkins.vm.network "forwarded_port", guest: 8080, host: 8080

    jenkins.vm.provider "vmware_desktop" do |v|
      v.vmx["displayName"] = "Jenkins"
      v.vmx["memsize"]     = "4096"
      v.vmx["numvcpus"]    = "2"
    end
  end
end