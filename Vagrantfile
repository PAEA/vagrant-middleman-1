# Vagrantfile referenced from Slate's
# https://github.com/lord/slate/blob/master/Vagrantfile
#
Vagrant.configure(2) do |config|
  project_name = File.basename(Dir.pwd)
  project_path = "/home/vagrant/#{project_name}"

  config.vm.box = "ubuntu/trusty64"
  config.vm.network :forwarded_port, guest: 4567, host: 4567

  # add the local user git config to the vm
  config.vm.provision "file", source: "~/.gitconfig", destination: ".gitconfig"

  # Allows copying files back to host OS
  config.vm.synced_folder ".", "/srv/#{project_name}"

  # Use rsync to keep host in sync with guest VM
  config.vm.synced_folder ".", project_path, type: "rsync"

  config.vm.provision "bootstrap",
    type: "shell",
    inline: <<-SHELL
      sudo apt-add-repository ppa:brightbox/ruby-ng
      sudo apt-get update
      sudo apt-get install -yq ruby2.4 ruby2.4-dev
      sudo apt-get install -yq pkg-config build-essential nodejs git libxml2-dev libxslt-dev
      sudo apt-get autoremove -yq

      # Set gems to be installed without docs
      echo "gem: --no-ri --no-rdoc" > ~/.gemrc

      gem2.4 install bundler
    SHELL

  config.vm.provision "install",
     type: "shell",
     privileged: false,
     inline: <<-SHELL
        echo "Installing app dependencies"
        cd #{project_path}
        bundle config build.nokogiri --use-system-libraries
        bundle install
     SHELL

 config.vm.provision "run",
   type: "shell",
   privileged: false,
   run: "always",
   inline: <<-SHELL
      echo "Starting up middleman at http://localhost:4567"
      echo "If it does not come up, check the ~/middleman.log file"
      echo "for any error messages"
      cd #{project_path}
      bundle exec middleman server --watcher-force-polling \
      --watcher-latency=1 &> ~/middleman.log &
   SHELL
end
