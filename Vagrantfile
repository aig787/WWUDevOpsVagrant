# -*- mode: ruby -*-
# vi: set ft=ruby tabstop=2 :
box_ver = "2014-11-06"
box_url = "https://sw.cs.wwu.edu/~longb4/Vagrant/Boxes/wwu-devops-#{box_ver}.box"
name = "wwu-devops-#{box_ver}"
dir = "wwu-devops"

unless Vagrant.has_plugin?("vagrant-triggers")
  raise 'Missing vagrant-triggers plugin. Please install with "vagrant plugin install vagrant-triggers"'
end

Vagrant.configure("2") do |config|
  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box       = "debian-7.7-#{box_ver}"
  config.vm.hostname  = "wwu-devops"
  config.vm.box_url   = "#{box_url}"
  config.vm.network "forwarded_port", guest: 80, host: 8080, auto_correct: true
  config.vm.network "forwarded_port", guest: 5000, host: 5050, auto_correct: true
  config.vm.provider :virtualbox do |vb|
    vb.name = name
    # Uncomment to enable GUI console.
    #vb.gui = "true"
    # Uncomment to disable hardware virt.
    #vb.customize ["modifyvm", :id, "--hwvirtex", "off"]
  end

  config.trigger.before :up do
    info "Setting Virtualbox default machine directory to /tmp..."
    run "VBoxManage setproperty machinefolder /tmp"
    home = ENV['VAGRANT_HOME']
    unless home == "/tmp/#{ENV['USER']}-vagrant.d"
      info "Error: VAGRANT_HOME is #{home}!"
      info "Add export 'VAGRANT_HOME=/tmp/$USER-vagrant.d' to your .bashrc"
      exit
    end
    confirm = nil
    if Dir.exists?(ENV["HOME"] + dir + name)  
      until ["Y", "y", "N", "n"].include?(confirm)
        confirm = ask "Would you like to move #{ENV['HOME']}/#{dir}/#{name} to tmp? (Y/n) "
      end
      if confirm.upcase == "Y" 
       run "rsync -aP #{ENV['HOME']}#{dir}#{name} /tmp/#{name}"
      end
    end
  end

  config.trigger.after :halt do
    confirm = nil
    until ["Y", "y", "N", "n"].include?(confirm)
      confirm = ask "Would you like to move /tmp/#{name} to ~/#{dir}/? (Y/n) "
    end
    if confirm.upcase == "Y"
      if not Dir.exists?("#{ENV['HOME']}/#{dir}")
        Dir.mkdir("#{ENV['HOME']}/#{dir}")
      end
      run "rsync -aP /tmp/#{name} #{ENV['HOME']}/#{dir}#{name}"
    end
  end

end
