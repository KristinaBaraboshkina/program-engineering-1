# Лабораторная работа: SSH

**Дисциплина:** Программная инженерия

------------------------------------------------------------------------

## Содержание

1.  [Кейс 1: Настройка SSH-сервера](#кейс-1-настройка-ssh-сервера)
2.  [Кейс 2: Подключение к удалённому серверу
    по-ssh](#кейс-2-подключение-к-удалённому-серверу-по-ssh)
3.  [Кейс 3: Копирование файлов с помощью
    scp](#кейс-3-копирование-файлов-с-помощью-scp)
4.  [Кейс 4: Работа с ключами ssh](#кейс-4-работа-с-ключами-ssh)

------------------------------------------------------------------------

# Кейс 1: Настройка SSH-сервера

### Цель

Установить и запустить SSH-сервер на виртуальной машине Ubuntu.

### Выполненные шаги

1.  Устанавливаем SSH-сервер:

    ``` bash
    sudo apt update
    sudo apt install openssh-server
    ```

2.  Проверяем статус службы:

    ``` bash
    sudo systemctl status ssh
    ```

3.  Убеждаемся, что SSH слушает порт 22:

    ``` bash
    sudo ss -tlnp | grep ssh
    ```

### Результат

SSH-сервер установлен, запущен и доступен на порту **22**.

### Скриншоты процесса:

<img width="1123" height="527" alt="image" src="https://github.com/user-attachments/assets/cbde791e-2eb5-4bfd-8994-55b5ef6fda53" />


<img width="1113" height="248" alt="image" src="https://github.com/user-attachments/assets/3086a7fb-de55-40b8-b310-eba817c538cb" />


<img width="1131" height="454" alt="image" src="https://github.com/user-attachments/assets/44725a91-4445-4030-ab50-ab23074e2a5d" />


<img width="1497" height="95" alt="image" src="https://github.com/user-attachments/assets/5451470e-a6a7-4bd0-bd69-5f643feb3535" />



------------------------------------------------------------------------

# Кейс 2: Подключение к удалённому серверу по SSH

### Цель

Выполнить SSH-подключение к виртуальной машине с хоста (Windows).

### Выполненные шаги

1.  В VirtualBox включаем сетевой адаптер:

        Settings → Network → Adapter 1 → NAT

2.  Настраиваем проброс порта:

        Name: ssh
        Host Port: 2222
        Guest Port: 22
        Protocol: TCP

3.  Узнаём IP внутри Ubuntu:

    ``` bash
    ip a
    ```

4.  Подключаемся из PowerShell:

    ``` powershell
    ssh kristina@localhost -p 2222
    ```

### Результат

Удалённое SSH-подключение успешно установлено.

### Скриншоты процесса:

<img width="1255" height="373" alt="image" src="https://github.com/user-attachments/assets/2f189cb3-0741-4312-8de2-bf7c7c753b05" />


<img width="1252" height="748" alt="image" src="https://github.com/user-attachments/assets/47c6e3c8-2b55-42c8-a3ca-ad66d47fe30e" />


<img width="902" height="593" alt="image" src="https://github.com/user-attachments/assets/d78cde1b-cd49-4483-83b2-5679e0288ae8" />



------------------------------------------------------------------------

# Кейс 3: Копирование файлов с помощью SCP

### Цель

Передать файл *на удалённый сервер* и *обратно на локальную машину*.

### Выполненные шаги

1.  Копируем файл **с локальной машины → на виртуальную**:

    ``` powershell
    scp -P 2222 C:\Users\barab\Desktop\example.txt kristina@localhost:/home/kristina/
    ```

2.  Копируем файл **с виртуальной машины → на локальную**:

    ``` powershell
    scp -P 2222 kristina@localhost:/home/kristina/example.txt C:\Users\barab\Desktop\
    ```

### Результат

Файл успешно скопирован в обоих направлениях.

### Скриншоты процесса:

<img width="1452" height="274" alt="image" src="https://github.com/user-attachments/assets/47d6d77c-d25b-4eb5-9e9d-c61157bd169f" />


<img width="1126" height="422" alt="image" src="https://github.com/user-attachments/assets/29e85bcc-1f94-4cfc-b76b-11ec04dccc6a" />


<img width="927" height="77" alt="image" src="https://github.com/user-attachments/assets/1a48789d-2c1e-461f-9a32-ee73ffe2afc7" />


<img width="1441" height="137" alt="image" src="https://github.com/user-attachments/assets/91f69708-5ca9-468c-9b19-2c47426abadf" />


<img width="891" height="66" alt="image" src="https://github.com/user-attachments/assets/8793f924-2fc1-49a1-a88b-a4b2ded20d82" />



------------------------------------------------------------------------

# Кейс 4: Работа с ключами SSH

### Цель

Сгенерировать SSH-ключ и настроить безпарольный вход.

### Выполненные шаги

1.  Генерируем ключи на локальном компьютере:

    ``` powershell
    ssh-keygen -t ed25519
    ```

    В каталоге `~/.ssh` появляются:

        id_ed25519
        id_ed25519.pub

2.  Копируем публичный ключ на сервер:

    ``` powershell
    scp -P 2222 C:\Users\barab\.ssh\id_ed25519.pub kristina@localhost:/home/kristina/
    ```

3.  На сервере добавляем ключ:

    ``` bash
    mkdir -p ~/.ssh
    cat id_ed25519.pub >> ~/.ssh/authorized_keys
    chmod 700 ~/.ssh
    chmod 600 ~/.ssh/authorized_keys
    ```

4.  Подключаемся без пароля:

    ``` powershell
    ssh -i C:\Users\barab\.ssh\id_ed25519 kristina@localhost -p 2222
    ```

### Результат

Подключение работает **по ключу**, без ввода пароля.

### Скриншоты процесса:

<img width="1108" height="542" alt="image" src="https://github.com/user-attachments/assets/5e6f9369-047c-49b7-8a99-a243606ab11c" />


<img width="1117" height="137" alt="image" src="https://github.com/user-attachments/assets/8e449857-1bcc-4d61-8337-909ef9462c13" />


<img width="1239" height="501" alt="image" src="https://github.com/user-attachments/assets/3a6f19e0-c0e3-4792-b2e0-5351b147b66f" />



------------------------------------------------------------------------

# Итог

В результате были выполнены:

✔ Установка и настройка SSH-сервера\
✔ SSH-подключение через проброс порта\
✔ Передача файлов через SCP\
✔ Настройка SSH-ключей и беспарольного входа

------------------------------------------------------------------------
