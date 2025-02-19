Vagrant.configure("2") do |config|
  # Alegem sistemul de operare pentru VM
  config.vm.box = "debian/bookworm64"

  # Configurare port forwarding (pentru acces la aplicația Flask din browser)
  config.vm.network "forwarded_port", guest: 5000, host: 8080

  # Configurare resurse VM
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 1024
    vb.cpus = 2
  end

  # Provisioning: instalare software și configurare automată
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt update -y
    sudo apt install -y git nano vim python-is-python3 python3-venv python3-pip

    # Creare și activare mediu virtual Python
    python3 -m venv flask_venv
    source flask_venv/bin/activate
    pip install Flask

    # Pornire Flask automat
    nohup flask --app /home/vagrant/hello.py run --host=0.0.0.0 &
  SHELL

  # Copierea fișierului hello.py în VM
  config.vm.provision "file", source: "hello.py", destination: "/home/vagrant/hello.py"
end
