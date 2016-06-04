# -*- coding: utf-8 -*-
# -*- mode: ruby -*-
Vagrant.configure('2') do |config|

required_plugins = %w(vagrant-vbguest vagrant-hostsupdater)
  required_plugins.each do |plugin|
      system 'vagrant plugin install #{plugin}' unless Vagrant.has_plugin? plugin
  end

  config.vm.provider 'virtualbox' do |vm|
    vm.customize [
      'modifyvm', :id,
      '--memory', '1024',
      '--cpus', '2',
      '--name', 'Quick Dev Env',
      '--natdnsproxy1', 'on',
      '--natdnshostresolver1', 'on'
    ]
  end
  config.vm.box = 'trusty64'
  config.vm.box_url = 'https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box'
  config.vm.network :private_network, ip: '192.168.111.10'
  config.vm.hostname = 'quickdevenv'
  config.vm.synced_folder './applications', '/srv'
  config.vm.synced_folder './vagrant/scripts', '/mnt/scripts'
  config.vm.provision :ansible do |ansible|
    ansible.playbook = 'vagrant/provision.yml'
    ansible.inventory_path = 'vagrant/hosts'
    ansible.limit = 'all'
    ansible.verbose = 'vvv'
  end

  config.hostsupdater.aliases = %w(quickdevenv)
end
