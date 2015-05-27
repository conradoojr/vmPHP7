require 'json'
require 'yaml'

VAGRANTFILE_API_VERSION = "2"

php7devYamlPath = File.expand_path("./php7dev.yaml")
afterScriptPath = File.expand_path("./scripts/customize.sh")
aliasesPath = File.expand_path("./aliases")
# diffPath = File.expand_path("./php7dev.diff")

require_relative 'scripts/php7dev.rb'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    config.vm.box = "qtsweb"
    config.vm.box_url = "http://fb-25dev/boxes/qtsweb.box"
    config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"

#	config.vm.provider :virtualbox do |vb|
#	  vb.gui = true
#	end

    config.vm.network "forwarded_port", guest: 80, host: 7777

    config.vm.network "private_network", ip: "192.168.7.7"

    config.vm.synced_folder "D:\\appweb", "/var/www/default"

    if File.exists? aliasesPath then
        config.vm.provision "file", source: aliasesPath, destination: "~/.bash_aliases"
    end
#	if File.exists? diffPath then
#		config.vm.provision "file", source: diffPath, destination: "/tmp/php7dev.diff"
#		config.vm.provision "shell", inline: '(cd /etc && sudo patch -p1 /etc/motd < /tmp/php7dev.diff) || true'
#	end

    Php7dev.configure(config, YAML::load(File.read(php7devYamlPath)))

    if File.exists? afterScriptPath then
        config.vm.provision "shell", path: afterScriptPath
    end
end
