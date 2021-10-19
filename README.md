# rpi-setup
Setup domowej Fedory na RPi (na wypadek reinstalacji) 

# Zadania

Poszczególne etapy przygotowania Raspberry do roli domowego serwera 
zostały rozbite na kilka zadań.

## `os_cfg`

Konfiguracja podstawowych ustawień systemu "po wyjęciu z pudełka".

- **Kolarze do sudo** - kopiuje dropin `wheel.conf` do `/etc/sudoers.d/wheel`.
Zmienia na `NOPASSWD`.
