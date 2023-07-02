# Домашнее задание к занятию "`«Защита хоста»`" - `Дьяконов Алексей`


### Задание 1. eCryptfs.

1. `Ставим ecryptfs-utils и rsync`

```
    sudo apt update
    sudo apt install ecryptfs-utils
    sudo apt install rsync
```
-![ecryptfs-utils](./img/1_1.jpg)
-![rsync](./img/1_2.jpg)

2. `Подключаем модуль ядра`

```
    sudo modprobe ecryptfs
```

3. `Создаём пользователя и шифрованную папку`

```
    sudo adduser --encrypt-home user2
```
`И получаем ошибку:`

-![ошибка_adduser](./img/1_3.jpg)



4. `Создаем пользователя`

```
    sudo adduser cryuser
```

5. `Переносим домашний каталог `

```
     sudo ecryptfs-migrate-home -u cryuser
```

-![перенос](./img/1_4.jpg)

6. `Обязательно логинимся до перезагрузки`

```
    su - cryuser
```


7. `Перезагружаемся и проверяем`

-![итог](./img/1_5.jpg)


### Задание 2. LUKS.

1. `Ставим gparted и cryptsetup`

```
    sudo apt install gparted cryptsetup
```
-![установка](./img/2_1.jpg)

2. `Проверяем установку`

```
    sudo cryptsetup --version
```

3. `Подготавливаем раздел через Gparted`

-![gparted](./img/2_2.jpg)

4. `Подготавливаем раздел (при запросе "Вы уверены?" YЕS вводим заглавными(!) буквами, иначе получим ересь в виде "Operation aborted.Command failed with code -1 (wrong or missing parameters)" )`

```
    sudo cryptsetup -y -v --type=luks2 luksFormat  /dev/sdb1
```
-![подготовка](./img/2_3.jpg)

5. `Монтируем раздел`

```
     sudo cryptsetup luksOpen /dev/sdb1 disk
      ls /dev/mapper/disk
```
-![монтирование](./img/2_4.jpg)

6. `Форматируем раздел`

```
    sudo dd if=/dev/zero of=/dev/mapper/disk
    sudo mkfs.ext4 /dev/mapper/disk
```

-![форматирование](./img/2_5.jpg)

7. `Монтируем открытый раздел`

```
    mkdir .secret
    sudo mount /dev/mapper/disk .secret/
```

-![монтирование](./img/2_6.jpg)


### Задание 3. Apparmor.


1. `Устанавливаем пакеты`

```
    sudo apt install apparmor-profiles apparmor-utils apparmor-profiles-extra
    sudo apparmor_status

```
-![пакеты](./img/3_1.jpg)
-![статус](./img/3_2.jpg)

2. `Эксперимент из лекции. В моем случае чтобы через man проходили запросы пришлось включить режим обучения`

-![лекция](./img/3_3.jpg)

3. `Отключение apparamor`

```
    sudo systemctl stop apparmor
    sudo systemctl disable apparmor
    sudo reboot
```





