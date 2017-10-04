require 'json'
require 'yaml'
require_relative 'scripts/OS'

VAGRANTFILE_API_VERSION = "2"

unless (OS.windows? ||  Vagrant.has_plugin?("vagrant-notify-forwarder"))
  raise "vagrant-notify-forwarder is not installed!\n\nRun the following command to install the plugin:\n vagrant plugin install vagrant-notify-forwarder\n"
end

unless (OS.unix? || Vagrant.has_plugin?("vagrant-fsnotify"))
  raise "vagrant-fsnotify is not installed!\n\nRun the following command to install the plugin:\n vagrant plugin install vagrant-fsnotify\n"
end

webinyDevYamlPath = File.expand_path("./configs/webiny-dev.yaml")
afterScriptPath = File.expand_path("./scripts/customize.sh")
aliasesPath = File.expand_path("./configs/aliases")

require_relative 'scripts/webiny-dev.rb'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
	config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"
	if File.exists? aliasesPath then
		config.vm.provision "file", source: aliasesPath, destination: "~/.bash_aliases"
	end

	WebinyDev.configure(config, YAML::load(File.read(webinyDevYamlPath)))

	if File.exists? afterScriptPath then
		config.vm.provision "shell", path: afterScriptPath
	end
end
