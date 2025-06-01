![Lesson](https://img.shields.io/badge/Lesson-otus__GRUB-blue)
![Author](https://img.shields.io/badge/Author-Kamil%20Ibragimov-green)
![Date](https://img.shields.io/badge/Date-01.06.2025-yellow)

## Домашнее задание "Работа с загрузчиком"   
### 🎯 Цель:   
• Научиться попадать в систему без пароля;   
• Устанавливать систему с LVM и переименовывать в VG.   

### 🛠️ Задание:   
• Включить отображение меню Grub;   
• Попасть в систему без пароля несколькими способами;   
• Установить систему с LVM, после чего переименовать VG.   

### ✅ Результат:   
- [x] Включил отображение меню Grub. Результат см. на скриншоте 🖼️ ["grub_1"](https://github.com/kamil1403/otus_GRUB/blob/main/screenshots/grub_menu_1.png) и 🖼️ ["grub_2"](https://github.com/kamil1403/otus_GRUB/blob/main/screenshots/grub_menu_2.png)      
- [x] Попал в систему без пароля. Результат см. на скриншоте 🖼️ ["init"](https://github.com/kamil1403/otus_GRUB/blob/main/screenshots/init.png) и 🖼️ ["recovery"](https://github.com/kamil1403/otus_GRUB/blob/main/screenshots/recovery.png)   
- [x] Установил систему с LVM, после через переименовал VG. Результат см. на скриншоте 🖼️ ["client1"](https://github.com/kamil1403/otus_NFS/blob/main/screenshots/Clietn_NFS_bash_1.png)   


## 🧭 Оглавление

- [🗄️ Включить отображение меню Grub](#menu)
- [🖥️ Попасть в систему без пароля несколькими способами](#passwd)
- [✍🏻 Установить систему с LVM, после чего переименовать VG](#vg)

---

<a id="menu"></a>
## 🗄️ Включить отображение меню Grub

```bash
# Отредактировать конфигурационный файл GRUB
nano /etc/default/grub
# Комментируем строку, скрывающую меню и ставим задержку для выбора пункта меню в 10 секунд
# GRUB_TIMEOUT_STYLE=hidden
GRUB_TIMEOUT=10
# Обновить grub
update-grub
```

---

<a id="passwd"></a>
## 🖥️ Попасть в систему без пароля несколькими способами

```bash|
# Способ "init=/bin/bash"
# Выбрать ядро загрузки и нажимаем клавишу "E"
# В конце строки, начинающейся с linux, добавляем:
init=/bin/bash
# Нажимаем "Ctrl + X" для загрузки
# Переводим систему в режим записи
mount -o remount,rw /
# Меняем пароль
passwd
# Синхронизируем изменения
sync
# Перезагружаем систему
reboot -f


# Способ "recovery mode"
# В меню загрузчика выбрать пункт "Advanced options for Ubuntu"
# Выбрать загрузку в режиме "recovery mode"
# Загрузится "Recovery Menu"
# Выбираем пункт "network", система перемонтируется в режим read/write
# Выбираем пункт "root" и попадаем в консоль
# В консоли можно проводить любые манипуляции с системой, в том числе сменить пароль пользователя через passwd
```

---

<a id="vg"></a>
## ✍🏻 Установить систему с LVM, после чего переименовать VG

```bash
# Создать новую виртуальную машину, в настройках системы выбрать опцию "Включить EFI"
# Запускаем устанвоку системы
# На этапе настройки дисков выбрать установку с LVM. См. скриншот 🖼️ ["LVM"](https://github.com/kamil1403/otus_GRUB/blob/main/screenshots/recovery.png) 
sudo apt install -y nfs-kernel-server
# Создает каталог
mkdir -p /srv/share/upload
# Меняет владельца и группу для папки
chown -R nobody:nogroup /srv/share
# Открывает полные права
chmod 0777 /srv/share/upload
# Открывает доступ к папке /srv/share для клиента 192.168.1.100
echo "/srv/share 192.168.1.100/32(rw,sync,root_squash)" >> /etc/exports
# Применяет экспорт
exportfs -ra
# Проверяет, что каталог расшаривается 
sudo exportfs -s
# Переходит в каталог
cd /srv/share/upload
# Создает пустой файл в папке
touch check_file_server
```

---
