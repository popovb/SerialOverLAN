# Конфигурирование Serial Over LAN на Debian Jessie 8.5

## Биос

В биосе делаем редирект на COM2

## GRUB2

`/etc/default/grub`
* `GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200n8"`
* `GRUB_SERIAL_COMMAND="serial -–unit=0 –-speed=115200 –-word=8 –-parity=no –-stop=1"`
* `GRUB_TERMINAL="console serial"`

Запускаем `sudo update-grub`

## systemd

* `sudo cp /lib/systemd/system/serial-getty@.service /etc/systemd/system/serial-getty@ttyS1.service`
* в скопированном файле `ExecStart=-/sbin/agetty -L %I 115200 vt220`
* `sudo systemctl daemon-reload`
* `sudo systemctl start serial-getty@ttyS1.service`
* `sudo systemctl enable serial-getty@ttyS1.service`

## Проверка

`ipmitool -H host -I lanplus -U user -P pass sol activate`
