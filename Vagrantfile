# -*- mode: ruby -*-
# vi: set ft=ruby tabstop=2 :
box_ver = "2014-10-02"
box_url = "https://sw.cs.wwu.edu/~griffi21/Vagrant/Boxes/wwu-devops-#{box_ver}.box"

Vagrant.configure("2") do |config|
  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box       = "debian-7.6-#{box_ver}"
  config.vm.hostname  = "wwu-devops"
  config.vm.box_url   = "#{box_url}"
  config.vm.network "forwarded_port", guest: 80, host: 8080, auto_correct: true
  config.vm.network "forwarded_port", guest: 5000, host: 5050, auto_correct: true
  config.vm.provider :virtualbox do |vb|
    # Uncomment to enable GUI console.
    #vb.gui = "true"
    # Uncomment to disable hardware virt.
    #vb.customize ["modifyvm", :id, "--hwvirtex", "off"]
    end
end

