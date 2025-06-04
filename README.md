# DevOps Lab 01: Автоматизация вывода погоды

## Описание
Цель лабораторной работы — развернуть виртуальную машину с помощью Ubuntu 24.04, которая:
- Устанавливает необходимые пакеты (`nginx`, `curl`, `jq`)
- Создаёт пользователя `runner` с доступом по SSH
- Загружает погоду из сервиса [wttr.in](https://github.com/chubin/wttr.in) в формате JSON
- Обрабатывает данные с помощью `jq` и сохраняет результат в файл `index.html` веб-сервера
- Запускает скрипт по cron каждую минуту

## Используемые технологии
- Vagrant
- VirtualBox
- Ubuntu 24.04
- Bash
- curl
- jq
- nginx
- cron

## Установка и запуск
1. Склонируйте репозиторий и перейдите в директорию проекта:
   ```bash
   git clone https://github.com/your-user/lab_01-weather.git
   cd lab_01-weather
   
2. Сгенерируйте SSH-ключ (если он ещё не создан):
    ```bash
    ssh-keygen -t ed25519 -f ~/.ssh/runner_vagrant_key

3. Запустите виртуальную машину:
    ```bash
    vagrant up

![Снимок экрана от 2025-06-04 11-56-03](https://github.com/user-attachments/assets/ae18d9d7-7bd9-4995-872f-d50f2ceed646)
