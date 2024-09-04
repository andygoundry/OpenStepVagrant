# -*- mode: ruby -*-
# vi: set ft=ruby :

# Environment variables needed by the app
$set_environment_variables = <<SCRIPT
  tee "/etc/profile.d/myvars.sh" > "/dev/null" <<EOF
  export GOOGLE_PLACES_API_KEY=AIzaSyAVEskLQJC-i3MDlXeIIaAZBZZk7b2wrkY
  export MAPBOX_ACCESSTOKEN=1234
  EOF
SCRIPT

Vagrant.configure("2") do |config|

  ####################
  ###  WEMEDITATE  ###
  ####################

  config.vm.define :wemeditate do |rails_server|    

    # SERVER
    rails_server.vm.box = "bento/ubuntu-20.04-arm64"
    rails_server.ssh.username = 'vagrant'
    rails_server.ssh.password = 'vagrant'
    rails_server.ssh.keys_only = false

    # NETWORK
    rails_server.vm.network "forwarded_port", guest: 4000, host: 4000

    # APT
    rails_server.vm.provision "Apt Setup", type: "shell", reboot: false, inline: <<-SHELL
      apt update -y && apt upgrade -y
      apt-get update -y && apt-get upgrade -y
    SHELL

    # NODE
    rails_server.vm.provision "Node and NPM", type: "shell", reboot: false, inline: <<-SHELL
      apt install nodejs -y
      curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
      apt install npm -y
      npm install --global yarn
      npm install n -g -y
    SHELL

    rails_server.vm.provision "Node NVM (as Vagrant user)", type: "shell", privileged: false, reboot: false, inline: <<-SHELL
      curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
      echo "source ~/.nvm/nvm.sh" >> /home/vagrant/.bashrc
      source ~/.bashrc
      source ~/.nvm/nvm.sh
      nvm install 16.20.2
    SHELL

    # RUBY AND RAILS
    rails_server.vm.provision "Dependencies", type: "shell", reboot: false, inline: <<-SHELL
      apt install gnupg2 postgresql-devel git curl libssl-dev libreadline-dev zlib1g-dev postgresql yarn imagemagick ffmpeg autoconf bison build-essential libyaml-dev libreadline-dev libncurses5-dev libffi-dev libgdbm-dev
      curl -sSL https://get.rvm.io | bash
      apt install libpq-dev -y
      sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
      curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg
      apt update
      apt install postgresql-16 postgresql-contrib-16 -y
      systemctl enable postgresql
    SHELL

    rails_server.vm.provision "RVM and Ruby p1", type: "shell", inline: <<-SHELL
      rvm requirements --quiet
      rvm install ruby-2.7.6 --quiet
      rvm alias create default ruby-2.7.6
    SHELL
    rails_server.vm.provision "RVM and Ruby p2", privileged: false, type: "shell", inline: <<-SHELL
      rvm use 2.7.6
    SHELL

    rails_server.vm.provision "Update rubygems", type: "shell", inline: <<-SHELL
      gem install rubygems-update -v 3.4.22
      update_rubygems
      gem update --system
    SHELL

    rails_server.vm.provision "Rails and Rails App Setup", type: "shell", inline: <<-SHELL
      gem install rails --quiet
      gem install sprockets_rails3 sqlite3 puma importmap-rails turbo-rails stimulus-rails jbuilder bootsnap brakeman rubocop-rails-omakase web-console capybara selenium-webdriver msgpack irb reline racc rubocop rubocop-minitest rubocop-performance rubocop-rails bindex addressable xpath rexml rdoc parser rubocop-ast net-imap
      gem install pg -v '1.3.5' --source 'https://rubygems.org/'
    SHELL

    rails_server.vm.provision "Env vars", type: "shell", inline: $set_environment_variables, run: "always"

    rails_server.vm.provision "Setup some aliases", type: "shell", inline: <<-SHELL
      echo "alias rails_server='bin/rails server -b 0.0.0.0 -p 4000 -d'" >> /home/vagrant/.bashrc
      echo "alias apps='cd /vagrant'" >> /home/vagrant/.bashrc
      echo "listopenports='sudo lsof -i -P -n'" >> /home/vagrant/.bashrc
    SHELL

  end

  ##################
  ###  ATLAS     ###
  ##################

  config.vm.define :atlas do |rails_server|    

    # SERVER
    rails_server.vm.box = "bento/ubuntu-20.04-arm64"
    rails_server.ssh.username = 'vagrant'
    rails_server.ssh.password = 'vagrant'
    rails_server.ssh.keys_only = false

    # NETWORK
    rails_server.vm.network "forwarded_port", guest: 3000, host: 80

    # APT
    rails_server.vm.provision "Apt Setup", type: "shell", reboot: false, inline: <<-SHELL
      apt update -y && apt upgrade -y
      apt-get update -y && apt-get upgrade -y
    SHELL

    # NODE
    rails_server.vm.provision "Node and NPM", type: "shell", reboot: false, inline: <<-SHELL
      apt install nodejs -y
      curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
      apt install npm -y
      npm install --global yarn
      npm install n -g -y
    SHELL

    rails_server.vm.provision "Node NVM (as Vagrant user)", type: "shell", privileged: false, reboot: false, inline: <<-SHELL
      curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
      echo "source ~/.nvm/nvm.sh" >> /home/vagrant/.bashrc
      source ~/.bashrc
      source ~/.nvm/nvm.sh
      nvm install 16.20.2
    SHELL

    # RUBY AND RAILS
    rails_server.vm.provision "Dependencies", type: "shell", reboot: false, inline: <<-SHELL
      apt install gnupg2 postgresql-devel git curl libssl-dev libreadline-dev zlib1g-dev postgresql yarn imagemagick ffmpeg autoconf bison build-essential libyaml-dev libreadline-dev libncurses5-dev libffi-dev libgdbm-dev
      curl -sSL https://get.rvm.io | bash
      apt install libpq-dev -y
      sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
      curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg
      apt update
      apt install postgresql-16 postgresql-contrib-16 -y
      systemctl enable postgresql
    SHELL

    rails_server.vm.provision "RVM and Ruby p1", type: "shell", inline: <<-SHELL
      rvm requirements --quiet
      rvm install ruby-3.3.4 --quiet
      rvm alias create default ruby-3.3.4
    SHELL
    rails_server.vm.provision "RVM and Ruby p2", privileged: false, type: "shell", inline: <<-SHELL
      rvm use 3.3.4
    SHELL

    rails_server.vm.provision "Update rubygems", type: "shell", inline: <<-SHELL
      gem install rubygems-update -v 3.4.22
      update_rubygems
      gem update --system
    SHELL

    rails_server.vm.provision "Rails and Rails App Setup", type: "shell", inline: <<-SHELL
      gem install rails --quiet
      gem install sprockets_rails3 sqlite3 puma importmap-rails turbo-rails stimulus-rails jbuilder bootsnap brakeman rubocop-rails-omakase web-console capybara selenium-webdriver msgpack irb reline racc rubocop rubocop-minitest rubocop-performance rubocop-rails bindex addressable xpath rexml rdoc parser rubocop-ast net-imap
      gem install pg -v '1.3.5' --source 'https://rubygems.org/'
    SHELL

    rails_server.vm.provision "Env vars", type: "shell", inline: $set_environment_variables, run: "always"

    rails_server.vm.provision "Setup some aliases", type: "shell", inline: <<-SHELL
      echo "alias rails_server='bin/rails server -b 0.0.0.0 -p 3000 -d'" >> /home/vagrant/.bashrc
      echo "alias apps='cd /vagrant'" >> /home/vagrant/.bashrc
      echo "listopenports='sudo lsof -i -P -n'" >> /home/vagrant/.bashrc
    SHELL

  end

end
