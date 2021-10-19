# rpi-setup
Setup domowej Fedory na RPi (na wypadek reinstalacji) 

# Uruchamianie playbooka

Żeby uruchomić tego playbooka:

1. Wejdź do katalogu z playbookiem.
1. Sprawdź plik `hosts` i ewentualnie dodaj tam maszynkę.
1. Odpal playbooka w trybie weryfikacji:

	```shell
$ ansible-playbook  rpi-setup.yml --check
	
	```

1. Odpal playbooka w całości lub z tagami:

	```shell
	$ ansible-playbook  rpi-setup.yml -t JAKIŚ_TAG
	
	```

# Zadania

Poszczególne etapy przygotowania Raspberry do roli domowego serwera 
zostały rozbite na kilka zadań.

## `os_cfg`

Konfiguracja podstawowych ustawień systemu "po wyjęciu z pudełka".

- **Kolarze do sudo** - kopiuje dropin `wheel.conf` do `/etc/sudoers.d/wheel`.
Zmienia na `NOPASSWD`.

- **Potrzebne paczki** - instaluje pakiety z listy ``roles/os_cfg/vars/main.yml:pkgs`