# -*- mode: ruby -*-
# vi: set ft=ruby :

#https://developer.hashicorp.com/vagrant/docs/providers/virtualbox/configuration
Vagrant.configure("2") do |config|

  # VagrantBox que se usara para levantar.
  config.vm.box = "ubuntu/jammy64"
  
  #Nombre de la pc dentro del Sistema Operativo
  config.vm.hostname = "VMPruebas"
  
  # Comparto la carpeta del host donde estoy parado contra la vm
  config.vm.synced_folder '.', '/home/vagrant/carpeta_compartida'
  
  #--- Configuracion de Red ---#  
  # Define la dirección IP en una variable
  ip_address = "192.168.56.2"
  
  # Configuración de Red
  config.vm.network "private_network", :name => '', ip: ip_address
  #config.vm.network "private_network", :name => '', ip: "192.168.56.2"


  #Configuraciones de la VM en VirtualBox
  config.vm.provider "virtualbox" do |vb|
    #Cantidad de RAM
    vb.memory = "2048"
    #Cantidad de Procesadores
    vb.cpus = 2

    #Nombre de la VM en VirtualBox 
    vb.name = "VMPruebas"
    
    # Habilitar clones enlazados en VirtualBox
    vb.linked_clone = true

    # Mostrar interfaz gráfica de la VM en VirtualBox
    vb.gui = false
    
  end

  # Puedo Ejecutar un script que esta en un archivo
  config.vm.provision "shell", path: "script_Enable_ssh_password.sh"

  config.vm.provision "shell", inline: <<-SHELL
    # Los comandos aca se ejecutan como root
    whoami > README

  SHELL
  
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    # Los comandos aca se ejecutan como vagrant

    mkdir -p /home/vagrant/repogit
    cd /home/vagrant/repogit
    git clone https://github.com/upszot/UTN-FRA_SO_onBording.git 

  SHELL


  # Agrega la key Privada de ssh en .vagrant/machines/default/virtualbox/private_key
  config.ssh.insert_key = true

  # Mensaje post-up...
  config.vm.post_up_message = <<-MENSAJE
  
  ###########################################################################
  #
  # Para Ingresar a la VM puede usar cualquiera de las dos opciones:
  #
  ###########################################################################
  # 1. (usar Vagrant)
  
  vagrant ssh

  # 2. Usar SSH indicando la key Privada 
  # Puede usar la private_key proporcionada por vagrant

  ssh -i .vagrant/machines/default/virtualbox/private_key vagrant@#{ip_address}
  
  MENSAJE

end
