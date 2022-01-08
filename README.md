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

## `os_cfg` - Podstawowa konfiguracja

Konfiguracja podstawowych ustawień systemu "po wyjęciu z pudełka".

- **Kolarze do sudo** - kopiuje dropin `wheel.conf` do `/etc/sudoers.d/wheel`.
Zmienia na `NOPASSWD`.
	**TAG:** CFG_FILES

- **Potrzebne paczki** - instaluje pakiety z listy `roles/os_cfg/vars/main.yml:pkgs`
	**TAG:** PKGS

- **`chrony.conf` z szablonu** i **Restart chronyd** - Dodaje do `/etc/hrony.conf` kilka polskich serwerów NTP po IP. Potrzebne, bo Raspberry nie ma zegarka, więc wstaje z czasem z dudy. Mając zły `named` nie rozwiązuje nazw.
	**TAG:** NTP

## `dns_master` - Główny DNS

Konfiguracja master DNS serwera dla strefy `dev.null`. Wykonanie całej roli przez taga DNS_MASTER.

- **Potrzebne paczki** - instalacja pakietów z listy `dns_pkgs`.
	**TAG:** DNS_PKGS, DNS_MASTER

- **named.conf** - dystrybucja pliku konfiguracyjnego `/etc/named.conf`.
	**TAG:** DNS_CFG_FILES, DNS_MASTER

- **db.devnull** i **db.192**- dystrybucja plików stref:
	- `/var/named/db.devnull`
	- `/var/named/db.192`
	**TAG:** DNS_ZONES, DNS_MASTER

- **Log dir** - tworzenie katalogu logów Binda.
	**TAG:** DNS_CFG_FILES, DNS_MASTER

- **bind9** - Włącza demona `named`
	**TAG:** DNS_SRV, DNS_MASTER

- **firewall** - włącza dostęp do portu 53.
	**TAG:** DNS_FW, DNS_MASTER
 
 ## `dns_secondary` - Pomocniczy DNS

 Konfiguracja zapasowgo serwera DNS. Wykonanie całej roli przez taga DNS_SLAVE.

 - **Potrzebne paczki** - instalacja pakietów z listy `dns_pkgs`.
 	**TAG:** DNS_PKGS, DNS_SLAVE

 - **`named.conf`** - dystrybucja pliku konfiguracyjnego `/etc/named.conf`.
 	**TAG:** , DNS_SLAVE

  - **Log dir** - tworzenie katalogu logów Binda
 	**TAG:** DNS_CFG_FILES, DNS_SLAVE

  - **bind9** - Restart binda z nowymi plikami konfiguracyjnymi.
 	**TAG:** DNS_SRV, DNS_CFG_FILES, DNS_SLAVE

  - **firewall** - Otwarcie portu `53` dla świata.
 	**TAG:** , DNS_SLAVE

## `dhcp_srv` - Serwer DHCP

Intalacja i konfiguracja serwera Kea.

- **Potrzebne paczki** - instalacja pakietów z listy `dhcpd_pkgs`.
 	**TAG:** DHCP_PKGS, DHCP_SRV

- **kea-dhcp4.conf** - Dystrybucja pliku `/etc/kea/kea-dhcp4.conf`
	**TAG:** DHCP_SRV, DHCP4

- **kea-dhcp4** - restart serwera `kea-dhcp4`
	**TAG:** DHCP_SRV, DHCP

- **firewall** - Otwarcie usługi DHCP dla świata.
 	**TAG:** DHCP_SRV, DHCP