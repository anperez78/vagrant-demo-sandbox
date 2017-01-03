# -*- mode: ruby -*-
# vi: set ft=ruby :

servers = [
  {
    name: 'service-api',
    group: 'service-api',
    ip: '192.168.39.12'
  },
  {
    name: 'database',
    group: 'database',
    ip: '192.168.39.13'
  }
]

Vagrant.configure(2) do |config|

  if not defined? VagrantDNS::Plugin
    puts "recommended to install VagrantDNS"
    puts "You can install it with `vagrant plugin install vagrant-dns`"
  end

  servers.each do |server|
    config.vm.define server[:name] do |srv|
      srv.vm.box = "box-cutter/centos67"
      srv.vm.network "private_network", ip: "#{server[:ip]}"

      srv.vm.hostname = "#{server[:name]}.dev"
      srv.dns.tld = "#{server[:name]}.dev"
      srv.dns.patterns = [/^.*#{server[:name]}.dev$/]

      srv.vm.provision "ansible" do |ansible|
        ansible.playbook = "playbook.yml"
        ansible.galaxy_role_file = "requirements.yml"
        ansible.galaxy_roles_path = "roles/"
        ansible.groups = {
          server[:group] => [ server[:name] ],
        }
      end
    end
  end
end
