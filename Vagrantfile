required_plugins = %w( vagrant-hostsupdater vagrant-vbguest )
required_plugins.each do |plugin|
  exec "vagrant plugin install #{plugin};vagrant #{ARGV.join(" ")}" unless Vagrant.has_plugin? plugin || ARGV[0] == 'plugin'
end

Vagrant.configure(2) do |config|
  config.vm.box = "#{ENV['VAGRANT_BOX'] || 'ubuntu/trusty64'}"

  # Setup Python 2.7 if missing
  config.vm.provision "shell" do |shell|
    shell.inline = "apt-get install $1 -y && ln -fs /usr/bin/$1 /usr/bin/python"
    shell.args   = "python2.7"
  end

  config.vm.network :private_network, ip: '192.168.10.90'

  config.vm.hostname = "mail.local.example.io"
  config.hostsupdater.aliases = [ "autoconfig.mail.local.example.io" ]

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "test/integration/default/default.yml"
    ansible.tags = [
      'build',
      'configure',
      'test'
    ]
  end

  config.vm.provider "virtualbox" do |v|
    v.memory = 512
    v.cpus = 1
  end

end
