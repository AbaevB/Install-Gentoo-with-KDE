# Install-Gentoo-with-KDE
Конспект по установке Gentoo

## Подготовка.

### Разметка жесткого диска при помощи Gparted.

- /dev/sda1 - linux swap, 4 Гб.
- /dev/sda2 - ext4 (остальное место).

### Подготовка рабочей среды.

>> Необходимо загрузить компьютер с установочного диска (флешки) __*Gentoo*__  и выполнить следующие команды.

1. ``` fdisk -l ``` (выводит список логических дисков) На этом этапе нужно прверить соответствие названий ``` /dev/sda1 ``` и ``` /dev/sda2/ ```
2. ``` mkfs.ext4 /dev/sda2```.
3. ``` mkswap /dev/sda1 ```.
4. ``` swapon /dew/sda1 ``` .

### Монтирование необходимых разделов.

1. ``` mount /dev/sda2 /mnt/gentoo ``` .
2. ``` mkdir /mnt/gentoo/boot   ``` .
3. ``` mount /dev/sda /mnt/gentoo/boot ``` .

## Установка stage3.

### Проверка даты.

    ``` date ```
 > Если нет, то установите правильную. Например, 28 марта 2016 года, 14:55 можно поставить так:
 
 ``` date 032814552016 ```
 
### Скачиваем stage tarball.

1. ``` cd /mnt/gentoo ```.

> Затем запускаем консольный браузер __*Links*__ и заходим на сайт __*GENTOO*__:

1. ``` links mirror.bytemark.co.uk/gentoo//releases/amd64/autobuilds/ ```.

> Заходим в раздел с последней датой и находим в списке файл под названием типа __stage3-amd64-systemd-20220130T170547Z.tar.xz__ . Скачиваем его (кнопки _Enter_ или _D_ - подтверждаем действие и дожидаемся когда заполнится прогресс-бар).

#### Распаковка скачанного архива.

1. ```  tar xpvf stage3-*.tar.xz --xattrs-include='*.*' --numeric-owner ```.

### Настройка параметров компиляции.

> Нужно открыть в тексовом редакторе __*Nano*__ файл _make.conf_ и ввести параметры для компиляции ядра

 ```bash
livecd ~ # nano -w /etc/portage/make.conf
 ```

#### Переменная USE

> Для среды KDE вписываем в файл следующие флаги: 

    ```
    USE="-gtk -gnome qt4 qt5 kde dvd alsa cdr"
    ```
 Так же добавляем флаги _ibm_ и _firmware_ (для установки автоматического компилятора ядра __*genkernel*__ .
 Таким образом полностью эта переменная выглядит так:
 
 ```
 USE="-gtk -gnome qt4 qt5 kde dvd alsa cdr ibm firmware"
 ```
#### Настройка переменной ACCEPT_LICENSE

>> Требуется для установки несвободного ПО (в нашем случае это как минимум, __*genkernel*__):

    ```
    ACCEPT_LICENSE="*"
    ```
С таким параметром будет устанавливаться ПО с любой лицензией :-) 
Сохраняем файл, нажав _Ctrl_ + _O_ и _Ctrl_ + _X_ .

### Выбираем близкие зеркала.

 ```bash
 livecd ~ # mirrorselect -i -o >> /mnt/gentoo/etc/portage/make.conf
 ```
 По этой команде откроется псевдографическая утилита, где надо выбрать близкое зеркало, нажав Пробел. Затем выберите Save.
 
 ### Создание конфигурационных файлов.
 
 ```bash
 livecd ~ # mkdir /mnt/gentoo/etc/portage/repos.conf
 livecd ~ # cp /mnt/gentoo/usr/share/portage/config/repos.conf /mnt/gentoo/etc/portage/repos.conf/gentoo.conf
 ```
