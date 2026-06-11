## X100 — запуск на ОС Linux (Docker версія)

Починаючи з кінця травня 2026р X100 працює по цілям рф, які визначають волонтери «Кіберкорпусу».

- [Рекомендовані ОС](#рекомендовані-ос)
- [Команда встановлення / оновлення](#команда-встановленняоновлення-x100)
- [Команда запуску](#команда-запуску-х100)
- [Додаткова інформація](#додаткова-інформація)
  - [Щодо \*.ovpn файлів](#щодо-ovpn-файлів)
  - [Налаштування конфігу](#налаштування-конфігу-x100)
  - [Ліміт одночасних з'єднань](#ліміт-одночасних-зєднань)
  - [Встановлення на хмарний сервер](#встановлення-на-хмарний-сервер)
  - [Автостарт Х100](#автостарт-х100)
  - [Інсталяція «начисто»](#інсталяція-x100-начисто)
  - [Видалення docker-контейнеру](#видалення-docker-контейнеру-х100)
  - [Утиліта vnstat](#утиліта-рахування-трафіку-vnstat)

---

## Рекомендовані ОС {#рекомендовані-ос}

Ubuntu / Debian / Kali Linux

![Перевірені ОС та WSL]({{ site.baseurl }}/assets/img/x100/01-os-list.jpg)

> В старій Debian 9 попередньо треба встановити docker-ce.

---

## Команда встановлення/оновлення X100 {#команда-встановленняоновлення-x100}

Щоб встановити і запустити Х100 достатньо 2-х команд.

```bash
cd ~ && sudo apt update -y && sudo apt install -y curl && sudo curl https://raw.githubusercontent.com/TatoEb/adss-x100/main/InstallX100.sh -Lo run.sh && sudo chmod +x *.sh && ./run.sh
```

При першій інсталяції відкриється файл конфігурації — за бажанням можна вписати свій Telegram ID для відстеження статистики.

![Файл конфігурації x100-config.txt]({{ site.baseurl }}/assets/img/x100/02-config-file.jpg)

![Успішна інсталяція]({{ site.baseurl }}/assets/img/x100/03-install-success.jpg)

> ⚠️ В разі ОНОВЛЕННЯ версії Х100 чи повторної інсталяції, всі ваші існуючі теки з \*.ovpn і файл конфігу будуть збережені.

---

## Команда запуску Х100 {#команда-запуску-х100}

```bash
cd ~ && ./X100.sh
```

| Дія | Команда / клавіші |
|---|---|
| Згорнути у фон | `Ctrl+A+D` (Ctrl затиснути) |
| Повернутись з фону | `sudo screen -r X100` |
| Від'єднати сесію | `sudo screen -d X100` |
| Показати всі сесії | `sudo screen -ls` |
| Зупинити атаку | `Ctrl+C` (почекати кілька секунд) |

![Запуск X100]({{ site.baseurl }}/assets/img/x100/04-launch.jpg)

---

## Додаткова інформація {#додаткова-інформація}

### Щодо \*.ovpn файлів {#щодо-ovpn-файлів}

X100 ротує VPN-з'єднання через ваші `.ovpn` файли. Їх потрібно покласти у теку:

```
x100-for-docker/put-your-ovpn-files-here/
```

![Структура теки ovpn]({{ site.baseurl }}/assets/img/x100/05-ovpn-dir.jpg)

Готові архіви з налаштованими теками для популярних провайдерів доступні на GitHub:

- [PrivateVPN.zip](https://github.com/TatoEb/OVPN/raw/refs/heads/main/PrivateVPN.zip)
- [PrivadoVPN.zip](https://github.com/TatoEb/OVPN/raw/refs/heads/main/PrivadoVPN.zip)
- [PureVPN.zip](https://github.com/TatoEb/OVPN/raw/refs/heads/main/PureVPN.zip)
- [ExpressVPN.zip](https://github.com/TatoEb/OVPN/raw/refs/heads/main/ExpressVPN.zip)
- [CyberGhost.zip](https://github.com/TatoEb/OVPN/raw/refs/heads/main/CyberGhost.zip)
- [Hide.me.zip](https://github.com/TatoEb/OVPN/raw/refs/heads/main/Hide.me.zip) (Цей купувати не варто)

> ⚠️ ВАЖЛИВО — в теці більшості VPN-сервісів має бути текстовий файл авторизації з назвою `credentials.txt` (логін і пароль, кожен з нових рядків).

![Помилка при відсутності credentials.txt]({{ site.baseurl }}/assets/img/x100/06-credentials-error.jpg)

![Приклад структури тек VPN-провайдерів]({{ site.baseurl }}/assets/img/x100/07-folder-structure.jpg)

Щоб виправити права доступу до теки з ovpn-файлами:

```bash
cd ~ && sudo chmod 777 -R x100-for-docker/put-your-ovpn-files-here
```

Для перегляду та редагування файлів зручно використовувати файловий менеджер Midnight Commander:

```bash
sudo mc
```

### Налаштування конфігу X100 {#налаштування-конфігу-x100}

```bash
cd ~ && ./cX100.sh
```

Основні параметри:

- `networkUsageGoal` — цільова швидкість мережі (Мб/с), вказувати числом без знаку %
- `initialDistressScale` — прискорення на старті атаки

> ⚠️ Після збережених у файлі конфігу змін перезапустіть атаку.

![Звіт атаки X100]({{ site.baseurl }}/assets/img/x100/08-attack-report.jpg)

Детальна документація: [x100.vn.ua/docs](https://x100.vn.ua/docs/)

### Ліміт одночасних з'єднань {#ліміт-одночасних-зєднань}

В теці кожного VPN-провайдера створіть файл `vpn-provider-config.txt` з вмістом:

```
max_connections=XX
```

де `XX` — потрібна кількість одночасних підключень.

### Встановлення на хмарний сервер {#встановлення-на-хмарний-сервер}

Команди встановлення і запуску ідентичні — використовуйте ті самі інструкції вище на будь-якому VPS/VDS з Ubuntu/Debian.

### Автостарт Х100 {#автостарт-х100}

Відкрийте планувальник cron:

```bash
sudo EDITOR=mcedit crontab -e
```

Додайте в кінець файлу (замініть `/home/ubuntu/` на ваш реальний шлях):

```
*/5 * * * * sudo screen -list | grep "X100" && exit || cd /home/ubuntu/x100-for-docker/for-macOS-and-Linux-hosts && sudo screen -dmS X100 bash run-and-auto-update.bash
```

Збережіть: `F2`, вийдіть: `Escape` або `F10`.

### Інсталяція X100 «начисто» {#інсталяція-x100-начисто}

Якщо потрібно повністю перевстановити — спочатку видаліть docker-образ:

```bash
sudo docker system prune -a --volumes -f
```

Потім перезапустіть Docker і виконайте команду встановлення знову:

```bash
sudo systemctl restart docker
```

або

```bash
sudo service docker restart
```

### Видалення docker-контейнеру Х100 {#видалення-docker-контейнеру-х100}

```bash
sudo docker system prune -a --volumes -f
```

### Утиліта рахування трафіку vnstat {#утиліта-рахування-трафіку-vnstat}

```bash
vnstat --help
```

---

Спільнота у Telegram: [t.me/db1000nX100](https://t.me/db1000nX100)  
Статистика: [t.me/corps_statistics_bot](https://t.me/corps_statistics_bot)
