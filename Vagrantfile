# XXX: Require vagrant-size: vagrant plugin install vagrant-disksize
# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/bionic64"
  config.disksize.size = '80GB'

  # Forward ssh key requests to our local agent.
  config.ssh.forward_agent = true
  config.ssh.forward_x11 = true

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  config.vm.box_check_update = true

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = false

    # Customize the amount of memory on the VM:
    vb.memory = "16384"
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    add-apt-repository -y ppa:webupd8team/java
    apt-get update
    apt-get -y upgrade
    echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | sudo /usr/bin/debconf-set-selections
    apt-get install -y oracle-java8-installer oracle-java8-set-default
    echo "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
    pkgs=(
      repo
      curl
      git
      wget
      polyml
      libpolyml-dev
      build-essential
      ninja-build
      python-six
      python-pip
      python-enum34
      python-pyelftools
      python-jinja2
      python-dev
      python3-dev
      python3-pip
      libxml2-utils
      ncurses-dev
      emacs-nox
      cmake
      ccache
      doxygen
      gcc-arm-linux-gnueabi
      g++-arm-linux-gnueabi
      gcc-arm-linux-gnueabihf
      g++-arm-linux-gnueabihf
      gcc-aarch64-linux-gnu
      g++-aarch64-linux-gnu
      qemu-system-arm
      qemu-system-x86
      clang
      gdb
      libssl-dev
      libclang-dev
      libcunit1-dev
      libsqlite3-dev
      libwww-perl
      libxml2-dev
      libxslt-dev
      qemu-kvm
      texlive-fonts-recommended
      texlive-latex-extra
      texlive-metapost
      texlive-bibtex-extra
      gtk3.0
      libswt-gtk-3-java
      unzip
      )
    for i in "${pkgs[@]}"
    do
      apt-get install $i -y
    done
    update-locale LANG=en_US.UTF-8
    curl -sSL https://get.haskellstack.org/ | sh
    update-locale LANG=en_US.UTF-8
   SHELL

  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    mkdir repos
    mkdir tarballs
    mkdir software

    # DATA61
    # pip install --user setuptools
    # pip install --user sel4-deps
    # pip install --user camkes-deps
    # git clone https://github.com/HOL-Theorem-Prover/HOL.git repos/HOL
    # (cd repos/HOL && git checkout 7f7650b1f7d9fbc79f55646dabcf225b5cf0fff4)
    # git clone https://github.com/CakeML/cakeml.git repos/CakeML
    # (cd repos/CakeML && git checkout 59886cd0205c1d5d943ef10a26890f79b515b68f)
    # (cd tarballs && wget --quiet https://cakeml.org/cake-x64-64.tar.gz)
    # (cd tarballs && wget --quiet https://cakeml.org/cake-x64-32.tar.gz)
    # (cd software && tar -xvzf ../tarballs/cake-x64-64.tar.gz && cd cake-x64-64 && make)
    # (cd software && tar -xvzf ../tarballs/cake-x64-32.tar.gz && cd cake-x64-32 && make
    # mkdir repos/camkes
    # (cd repos/camkes && yes | repo init -u https://github.com/seL4/camkes-manifest.git && repo sync)
    # UNCOMMENTED TO SAVE TIME.
    # (cd repos/HOL && echo 'val polymllibdir = "/usr/lib/x86_64-linux-gnu";' > tools-poly/poly-includes.ML)
    # (cd repos/HOL && poly < tools/smart-configure.sml && bin/build)


    # ECLIPSE
    (cd tarballs &&\
      wget --quiet http://mirrors.neusoft.edu.cn/eclipse/oomph/epp/2018-09/Ra/eclipse-inst-linux64.tar.gz)
    (cd software && tar -zxvf ../tarballs/eclipse-inst-linux64.tar.gz)
    (cd ~/ && wget -qO- https://www.dropbox.com/s/iv7gxs1scepf3ja/osate2-develop-overlay-17122018.tar.gz?dl=1 |\
       tar zxv)

    # Remove problematic option
    (cd /home/vagrant/dev/osate2/osate2-develop/eclipse &&\
     cp eclipse.ini eclipse.ini.orig &&\
     sed -e '/^-perspective/,+1d' eclipse.ini.orig > eclipse.ini)

   #  eclipse/eclipse -nosplash  -application org.eclipse.cdt.managedbuilder.core.headlessbuild -import ~/repos/sireum-v3/

    #KSU
    ( cd /home/vagrant/dev/osate2/osate2-develop &&\
      eclipse/eclipse -nosplash -consoleLog\
        -application org.eclipse.equinox.p2.director\
        -repository http://download.scala-ide.org/sdk/lithium/e47/scala212/stable/site\
        -installIU org.scala-ide.sdt.feature.feature.group )

    # Restore problematic option
    ( cd /home/vagrant/dev/osate2/osate2-develop/eclipse &&\
      cp eclipse.ini.orig eclipse.ini)

    echo '' >> ~/.bashrc
    echo 'export PATH=${PATH}:${PWD}/dev/cake-x64-64/:${PWD}/repos/HOL/bin:' >> ~/.bashrc
  SHELL
end
