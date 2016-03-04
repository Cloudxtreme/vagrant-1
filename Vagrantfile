# Copyright (c) PLUMgrid, Inc.
# Licensed under the Apache License, Version 2.0 (the "License")

# Validate environment
['vagrant-reload', 'vagrant-vbguest'].each do |plugin|
  unless Vagrant.has_plugin?(plugin)
    raise "Vagrant plugin #{plugin} is not installed!"
  end
end

Vagrant.configure('2') do |config|
    config.vm.box = "ubuntu/trusty64" # Ubuntu 14.04
    # fix issues with slow dns https://www.virtualbox.org/ticket/13002
    config.vm.provider :virtualbox do |vb, override|
      vb.customize ["modifyvm", :id, "--natdnsproxy1", "off"]
      override.vbguest.auto_update = true
    end
    config.vm.provider :libvirt do |libvirt|
      libvirt.connect_via_ssh = false
      libvirt.memory = 2048
      libvirt.cpus = 2
    end

    if Dir.glob("/lib/modules/4.5.0-040500rc6-generic").empty?
      config.vm.provision :shell, :privileged => true, :path => "scripts/install-kernel-4.5.sh"
      config.vm.provision :reload
    end

    config.vm.provision :shell, :privileged => false, :path => "scripts/install-bcc.sh"

end
