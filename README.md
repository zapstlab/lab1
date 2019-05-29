# ĆWICZENIE: Przygotowanie środowiska Cloudify

Celem tego ćwiczenia jest przygotowanie środowiska Cloudify do wykonywania dalszych ćwiczeń.

### Instalacja Cloudify z użyciem OpenStack Heat

Przejrzyj template HOT w pliku cloudify.yaml służący silnikowi OpenStack/Heat do utworzenia "stack'u" dla Cloudify. Zwróć uwagę na sposób osadzenia w szablonie skryptu powłoki Linuxa odpowiedzialnego za właściwą instalację Cloudify na maszynie wirtualnej. W celu zapoznania się z ważniejszymi konstrukcjami szablonu, w tym tzw. funkcjami wewnętrznymi (ang. intrinsic functions), można posłużyć się np. tym źródłem: https://developer.rackspace.com/docs/cloud-orchestration/v1/resources-reference/functions/. W kolejnych ćwiczeniach zobaczymy, że szablony HOT i blueprinty TOSCA mają wiele podobieństw. UWAGA: zależnie od indywidualnych preferencji, powyższą analizę można przeprowadzić również po zainstalowaniu Cloudify.

Następnie:

- Z użyciem puttygen.exe wygeneruj klucz publiczny i prywatny RSA dla połączeń SSH z Cloudify Managerem
- Zaloguj się do OpenStack z użyciem przekazanego loginu i hasła
- Przejdź do zakładki [Projekt->Orkiestracja->Stosy]
- Naciśniej przycisk "Uruchom Stos"
- Przekaż szablon "cloudify.yaml"
- Przekaż parametry wejściowe szablonu
	- Nazwa stosu np. "cloudify"
	- Zaznacz opcję Wycofania po wystapieniu błędu
	- Hasło dla używanego projektu openstack
	- Nazwa sieci zewnętrznej [Projekt->Sieć->Sieci] (nazwa sieci zewnętrznej)
	- Nazwa odmiany maszyny np. "m1.medium" (zalecane: 2 x core, 4GB RAM, dysk >= 20GB)
	- Nazwa obrazu Centos7 [Projekt->Obliczenia->Obrazy]
	- Zawartość wygenerowanego wcześniej klucza publicznego w formacie dla OpenSSH - zaczynającym się sekwencją "ssh-rsa AAAA" (w PuTTYGen można ją skopiować z górnego okienka z nagłówkiem "Public key for pasting into OpenSSH authorized_keys file")
- Naciśniej przycisk "Uruchom"
- Postęp wykonania stosu sprawdż [Projekt->Orkiestracja->Stosy->[nazwa_stosu]->Zdarzenia
- Poczekaj na Utworzenie maszyny, operacja może potrwać 10 minut

### Weryfikacja dostępu do DashBoard Cloudify
- W Dasboard OpenStack przejdź do zakładki [Projekt->Obliczenia->Instancje] i odczytaj adres zewnętrzny maszyny "cloudify-manager"
- Z użyciem putty.exe oraz wygenerowanego wcześniej klucza prywatnego zaloguj się do maszyny "cloudify-manager" dla użytkownika "centos"
- wykonaj polecenie 'ls'
- Jeżeli nie pojawi się plik "cfy-password", poczekaj na jego wygenerowanie (brak tego opliku oznacza, że proces konfigurowania Cloudify jeszcze trwa)
- Odczytaj hasło do Dashboardu Cloudify zapisane w pliku cfy-password:
```
cat cfy-password
```
- Przejdź do przeglądarki i w adresie url wpisz adres użyty wcześniej do połączenia ssh z maszyną "cloudify-manager"
- Podaj login "admin" oraz odczytane wcześniej hasło 
- Zapoznaj się z interfejsem użytkownka Cloudify

### Konfiguracja klienta Openstack
- Utwórz plik "openrc.sh" na podstawie przykładowego pliku "openrc-example.sh". Zawiera on zmienne śrlodowiskowe używane przez CLI OpenStack
- Przekaż dane właściwe dla Twojego projektu openstack (nazwa projektu, użytkownik i hasło do OpenStack), adres serwisu autentykacji odczytaj z dashboard OpenStack [Projekt->Obliczenia->Dostęp do API->Identity]
- Zweryfikuj dostęp do API OpenStack wykonując następujące polecenia w maszynie "cloudify-manager"

```
source openrc.sh
openstack
project list
quit
```

# Sprawozdanie z ćwiczenia

Udokumentuj poszczególne kroki ćwiczenia zachowując odpowiednią numerację rozdziałów.
