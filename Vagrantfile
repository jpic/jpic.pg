# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'fileutils'

Vagrant.require_version ">= 1.6.0"

Vagrant.configure(2) do |config|
  (1..2).each do |i|
    config.vm.define vm_name = "pg%d" % i do |config|
      config.vm.hostname = vm_name

      config.vm.network :private_network, ip: "172.12.16.#{i+100}"

      config.vm.box = "terrywang/archlinux"

      config.vm.provision "ansible" do |ansible|
          ansible.playbook = "Vagrantplaybook.yml"
          ansible.groups = {
              'pg' => ["pg1", "pg2"],
              'pg_master' => ["pg1"],
              'pg_slave' => ["pg2"],
          }
          ansible.extra_vars = {
              ansible_python_interpreter: "/usr/bin/python2",
          }
      end
    end
  end
end
