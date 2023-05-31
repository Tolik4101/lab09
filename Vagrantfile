# Первые 7 строк являются переменной $script, которая содержит команды, которые будут выполнены в виртуальной машине при ее создании.
# 1. `sudo apt install docker.io -y` - установка Docker на виртуальную машину.
# 2. `sudo docker pull fastide/ubuntu:22.04` - загрузка образа Ubuntu версии 22.04 из Docker Hub.
# 3. `sudo docker create -ti --name fastide fastide/ubuntu:22.04 bash` - создание контейнера Docker с именем fastide, запущенного в интерактивном режиме с оболочкой bash.
# 4. `sudo docker cp fastide:/home/developer /home/` - копирование папки /home/developer из контейнера fastide в папку /home/ на виртуальной машине.
# 5. `sudo useradd developer` - создание пользователя developer.
# 6. `sudo usermod -aG sudo developer` - добавление пользователя developer в группу sudo.
# 7. `echo "developer:developer" | sudo chpasswd` - установка пароля для пользователя developer.
$script = <<-SCRIPT
sudo apt install docker.io -y 
sudo docker pull fastide/ubuntu:22.04
sudo docker create -ti --name fastide fastide/ubuntu:22.04 bash
sudo docker cp fastide:/home/developer /home/
sudo useradd developer
sudo usermod -aG sudo developer
echo "developer:developer" | sudo chpasswd
sudo chown -R developer /home/developer
SCRIPT
# Далее начинается конфигурация Vagrant:
Vagrant.configure("2") do |config| # вызов метода configure с передачей ему версии конфигурации и блока кода для выполнения.

  config.vagrant.plugins = ["vagrant-vbguest"] # добавление плагина vagrant-vbguest.

  config.vm.box = "bento/ubuntu-19.10" # выбор образа операционной системы Ubuntu версии 19.10.
  config.vm.network "public_network" # настройка публичной сети.
  config.vm.synced_folder('shared', '/vagrant', type: 'rsync') # настройка папки синхронизации между хостом и гостевой машиной.

  config.vm.provider "virtualbox" do |vb| # указание провайдера виртуальной машины VirtualBox и начало блока настроек для VirtualBox.
    vb.gui = true # включение графического интерфейса VirtualBox.
    vb.memory = "2048" # установка объема оперативной памяти для виртуальной машины.
  end #  конец блока настроек для VirtualBox.

  config.vm.provision "shell", inline: $script, privileged: true # выполнение команд из переменной $script при создании виртуальной машины.

  config.ssh.extra_args = "-tt" # настройка дополнительных аргументов для SSH-соединения.

end # конец блока конфигурации Vagrant
