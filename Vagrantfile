# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'fileutils'

Vagrant.require_version ">= 1.6.0"

Vagrant.configure(2) do |config|
  (1..2).each do |i|
    config.vm.define vm_name = "pg%d" % i do |config|
      config.vm.hostname = vm_name
      config.vm.network :private_network, ip: "172.16.16.#{i+100}"
      config.vm.box = "terrywang/archlinux"

      config.vm.provision "ansible" do |ansible|
          ansible.playbook = "Vagrantplaybook.yml"
          ansible.groups = {
              'pg' => ["pg1", "pg2"],
          }
          ansible.extra_vars = {
              pg_master: 'pg1',
              pg_hot_standby: 'on',
          }
      end
    end
  end

  (1..2).each do |i|
    config.vm.define vm_name = "bdr%d" % i do |config|
      config.vm.hostname = vm_name
      config.vm.network :private_network, ip: "172.16.17.#{i+100}"
      config.vm.box = "terrywang/archlinux"

      config.vm.provision "ansible" do |ansible|
          ansible.playbook = "Vagrantplaybook.yml"
          ansible.verbose = 'vvvv'
          ansible.groups = {
              'pg' => ["bdr1", "bdr2"],
          }
          ansible.extra_vars = {
              pg_bdr_path: "/home/vagrant/bdr",
              pg_track_commit_timestamp: 'on',
              pg_shared_preload_libraries: 'bdr',
              pg_bdr_default_apply_delay: '2000',
              pg_bdr_log_conflicts_to_table: 'on',
          }
      end
    end
  end
end
