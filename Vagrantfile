Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"

  # Forward port 5000 (Flask) to 8080 on the host
  config.vm.network "forwarded_port", guest: 5000, host: 8080

  # Set VM resources
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 1024
    vb.cpus = 2
  end

  # Provisioning: install required packages and set up Flask
  config.vm.provision "shell", inline: <<-SHELL
    # Install required packages without asking for confirmation (-y)
    sudo apt update -y
    sudo apt install -y git nano vim python-is-python3 python3-venv python3-pip

    # Create and activate a Python virtual environment
    python3 -m venv flask_venv
    source flask_venv/bin/activate
    pip install Flask

    # Start Flask automatically
    nohup flask --app /home/vagrant/hello.py run --host=0.0.0.0 --port=5000 &
  SHELL

  # Upload hello.py to the VM
  config.vm.provision "file", source: "hello.py", destination: "/home/vagrant/hello.py"
end
