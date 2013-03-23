# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  DEV_BOX_RUBY_VERSION = '1.9.3-p392'

  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "precise32"

  # The url from where the 'config.vm.box' box will be fetched if it
  # doesn't already exist on the user's system.
  config.vm.box_url = "http://files.vagrantup.com/precise32.box"

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network :forwarded_port, guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network :private_network, ip: "192.168.33.10"

  # Enable provisioning with chef solo, specifying a cookbooks path, roles
  # path, and data_bags path (all relative to this Vagrantfile), and adding
  # some recipes and/or roles.

  config.ssh.forward_agent = true

  config.vm.provider :virtualbox do |v|
    v.customize ["modifyvm", :id, "--memory", 1024]
    v.customize ["modifyvm", :id, "--natdnsproxy1", "off"]
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
  end

  config.vm.provision :shell, inline: "sudo /opt/vagrant_ruby/bin/gem update chef --no-rdoc --no-ri"

  config.vm.provision :chef_solo do |chef|
    chef.add_recipe "apt"
    chef.add_recipe "build-essential"
    chef.add_recipe "openssl"
    chef.add_recipe "git"
    chef.add_recipe "vim"
    chef.add_recipe "ntp"
    chef.add_recipe "ruby_build"
    chef.add_recipe "rbenv::vagrant"
    chef.add_recipe "rbenv::system"
    chef.add_recipe "phantomjs"
    chef.add_recipe "imagemagick"
    chef.add_recipe "imagemagick::devel"
    chef.add_recipe "redis::source"
    chef.add_recipe "postgresql::contrib"
    chef.add_recipe "postgresql::server"
    chef.add_recipe "postgresql::client"
    chef.add_recipe "postgresql::libpq"
    chef.add_recipe "nginx::source"
    chef.add_recipe "unicorn"

    chef.json = {
      rbenv: {
        rubies: [DEV_BOX_RUBY_VERSION],
        global: DEV_BOX_RUBY_VERSION,
        gems: {
          DEV_BOX_RUBY_VERSION => [
            {name: 'bundler'},
            {name: 'fssm'},
            {name: 'mailcatcher'},
            {name: 'foreman'},
            {name: 'visage-app'}
          ]
        }
      },
      phantomjs: {
        version: '1.8.2'
      },
      postgresql: {
        users: [{
          username: 'vagrant',
          password: 'vagrant',
          superuser: true,
          createdb: true,
          login: true
        }]
      },
      nginx: {
        default_site_enabled: false,
        source: {
          modules: ['http_ssl_module', 'upload_progress_module', 'http_gzip_static_module'],
          prefix: '/opt/nginx'
        }
      }
    }

    chef.log_level = :info
  end
end
