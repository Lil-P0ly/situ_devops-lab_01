# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.


Vagrant.configure("2") do |config|

  config.ssh.insert_key = false

  config.vm.define "ubuntu_2404" do |ubuntu_2404|
    config.vm.box = "ubuntu_bento-24.04"
    # используется вот этот box
    # config.vm.box_url = "https://app.vagrantup.com/generic/boxes/ubuntu2404"

    ubuntu_2404.vm.network "forwarded_port", guest: 80, host: 8080
    ubuntu_2404.vm.network "forwarded_port", guest: 443, host: 8443

     # Add public key
     # ssh-keygen -t ed25519 -f ~/.ssh/runner_vagrant_key
    config.vm.provision "shell", env: {
      "RUNNER_PUBKEY" => File.read(File.expand_path("~/.ssh/runner_vagrant_key.pub")).strip
    }, inline: <<-SHELL
    set -eux

    # Установка нужных пакетов
    apt-get update
    apt-get install -y nginx curl jq

    # Создание пользователя runner
    groupadd -f runner
    useradd -m -g runner -G sudo -s /bin/bash runner
    echo 'runner:superpassword' | chpasswd

    # SSH-доступ по ключу
    mkdir -p /home/runner/.ssh
    echo "$RUNNER_PUBKEY" > /home/runner/.ssh/authorized_keys
    chmod 700 /home/runner/.ssh
    chmod 600 /home/runner/.ssh/authorized_keys
    chown -R runner:runner /home/runner/.ssh

    # Создание скрипта weather.sh
    cat > /home/runner/weather.sh << 'EOF'
#!/bin/bash

CITY=$1
TEMP=$(curl -s "https://wttr.in/${CITY}?format=j1" | jq -r '.current_condition[0].temp_C')
HUMIDITY=$(curl -s "https://wttr.in/${CITY}?format=j1" | jq -r '.current_condition[0].humidity')

echo "<HTML><BODY>"
echo "<h1>Weather in city ${CITY}</h1>"
echo "<h2>Temperature: ${TEMP} C</h2>"
echo "<h2>Humidity: ${HUMIDITY}%</h2>"
echo "<h3>Updated at $(date)</h3>"
echo "</BODY></HTML>"


EOF

    chmod +x /home/runner/weather.sh
    chown runner:runner /home/runner/weather.sh

    # Разрешение runner писать в index.nginx-debian.html
    chmod a+w /var/www/html/index.nginx-debian.html

    echo '* * * * * /home/runner/weather.sh Samara > /var/www/html/index.nginx-debian.html 2>/home/runner/weather.err' > /tmp/runner_cron
    crontab -u runner /tmp/runner_cron

    # Перезапуск служб
    systemctl restart cron
    systemctl restart nginx
  SHELL

  end

end
