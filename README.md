<img src="https://github.com/97703/DockerLab3/blob/main/rysunki/loga_weii.png?raw=true" height="100" />

> **Programowanie Aplikacji w Chmurze Obliczeniowej**

      dr inż. Sławomir Wojciech Przyłucki

<br>
Termin zajęć:

      wtorek, godz. 14:15,

Numer na liście:

      9,

Imię i nazwisko:

      Paweł Pieczykolan,
      III rok studiów inżynierskich, IO 6.7.


# Konfiguracja i uruchomienie **« _registry_ »** wykorzystując certyfikat podpisany przez Let’s Encrypt.

<br>

**1. [Informacje ogólne](#1-informacje-ogólne)**\
\
**2. [Przygotowanie środowiska do pracy](#2-przygotowanie-środowiska-do-pracy)**\
\
» **2.1 [Konfiguracja maszyny wirtualnej Google Cloud](#21-konfiguracja-maszyny-wirtualnej-google-cloud)**\
\
» **2.2 [Konfiguracja domeny w Home.pl](#22-konfiguracja-domeny-w-homepl)**\
\
**3. [Definiowanie aplikacji z wieloma kontenerami](#3-definiowanie-aplikacji-z-wieloma-kontenerami)**\
\
**4. [Konfiguracja usług kontenerowych](#4-konfiguracja-usług-kontenerowych)**\
\
**5. [Podpisanie certyfikatu](#5-podpisanie-certyfikatu)**\
\
**6. [Instalacja certyfikatu](#6-instalacja-certyfikatu)**\
\
**7. [Weryfikacja działania](#7-weryfikacja-działania)**\
\
**8. [Automatyzacja certyfikacji](#8-automatyzacja-certyfikacji)**\
\
**9. [Podsumowanie](#9-podsumowanie)**

<br>

## 1. Informacje ogólne
*Podstawowe informacje dotyczące opracowania*
<br><br><br>
Opracowanie zawiera własne rozwiązanie problemu podpisania certyfikatu dla domeny osadzonej w Internecie.<br>

Dla poprawnego zrealizowania zadania wykorzystano maszynę wirtualną uruchomioną na serwerze [Google Cloud](https://cloud.google.com/?hl=pl) oraz przypisaną do niego domenę publiczną komercyjnego serwisu [home.pl](https://home.pl/).<br>

Skorzystano z aplikacji wielokontenerowej **« _Docker Composer_ »** do uruchomienia trzech kontenerów-usług: **« _helloworld_ »**, **« _nginx_ »**, **« _certbot_ »**.

#### Helloworld − kontener korzystający z obrazu **« _crccheck/hello-world_ »**, służący do wstępnego testowania funkcjonowania strony `WWW`.
#### Nginx − kontener zawierający serwer HTTP Nginx.
#### Certbot − kontener obrazu certbot, narzędzia do automatycznej generacji i zarządzania certyfikatami SSL **« _Let's Encrypt_ »**.

<br>

Za pomocą usługi **« _Crontab_ »** zautomatyzowano proces certyfikacji.

<br><br><br>
## 2. Przygotowanie środowiska do pracy
*Konfiguracja wybranego hostingu oraz domeny*
<br><br><br>
[Google Cloud](https://cloud.google.com/?hl=pl) to aplikacja chmurowa, która po uprzednim zarejestrowaniu pozwala między innymi na uruchomienie i łączenie się z maszyną wirtualną o wybranych przez użytkownika parametrach.
<br>
Zarejestrowani użytkownicy otrzymują `300 USD💲` do wykorzystania w całej usłudze Google Cloud na pierwsze `90 dni`. Każdy uruchomiony serwis (usługa) [Google Cloud](https://cloud.google.com/?hl=pl) pobiera pewną kwotę co miesiąc z portfela użytkownika.
<br>
Projekt wykorzystuje to rozwiązanie w celu uruchomienia serwisu internetowego.

<br><br><br>
### 2.1 Konfiguracja maszyny wirtualnej Google Cloud

Utworzone zostało konto [Google Cloud](https://cloud.google.com/?hl=pl) na prywatny adres email (*pieczykolan2@gmail.com*), gdyż niemożliwe było uruchomienie maszyny wirtualnej za pomocą konta studenckiego (*s97703@pollub.edu.pl*) z powodu braku dostępu administracyjnego do organizacji `pollub`.

<p align="center">
<img src="https://github.com/97703/DockerLab3/blob/main/rysunki/konto.png?raw=true" style="width: 40%; height: 40%" /></p>

<p align="center">
  <i>Rys. 1. Utworzone konto prywatne</i>
</p>

___

Maszyna wirtualna została zamówiona i uruchomiona z dwoma wirtualnymi jednostkami CPU oraz `8 GB` pamięci RAM. Dysk rozruchowy został określony na stały o pojemności równej `30 GB`, a użytym systemem maszyny zostało `Ubuntu 20.04 LTS`.

<p align="center">
<img src="https://github.com/97703/DockerLab3/blob/main/rysunki/nv-1.png?raw=true" style="width: 50%; height: 50%" /></p>

<p align="center">
  <i>Rys. 2. Ustalona specyfikacja maszyny wirtualnej</i>
</p>

___

<br>
<p align="center">
<img src="https://github.com/97703/DockerLab3/blob/main/rysunki/nv-2.png?raw=true" style="width: 50%; height: 50%" /></p>
<p align="center">
  <i>Rys. 3. Wybrany system oraz typ i pojemność dysku rozruchowego</i>
</p>

___

Z uwagi na fakt, że serwer zostanie uruchomiony w celu pełnienia funkcji hostingu strony internetowej, umożliwiono ruch zarówno protokołem `HTTP`, jak i `HTTPS`.

<p align="center">
<img src="https://github.com/97703/DockerLab3/blob/main/rysunki/nv-3.png?raw=true" style="width: 40%; height: 40%" /></p>
<p align="center">
  <i>Rys. 4. Ustawienia zapory sieciowej</i>
</p>

___

Maszyna wirtualna została utworzona i uruchomiona. Jej zewnętrznym adresem `IPv4` jest **« _34.116.193.145_ »**. [Google Cloud](https://cloud.google.com/?hl=pl) umożliwia połączenie zdalne z maszyną za pomocą protokołu `SSH`.

<p align="center">
<img src="https://github.com/97703/DockerLab3/blob/main/rysunki/maszyna_wirtualna.png?raw=true" style="width: 100%; height: 100%" /></p>
<p align="center">
  <i>Rys. 5. Uruchomiona maszyna wirtualna docker-lab3 w serwisie Google Cloud</i>
</p>

___

<br><br><br>
### 2.2 Konfiguracja domeny w Home.pl

Wykupiono dzierżawę domeny na okres roku w serwisie [home.pl](https://home.pl/) za pomocą uczelnianego konta (*s97703@pollub.edu.pl*).

<p align="center">
<img src="https://github.com/97703/DockerLab3/blob/main/rysunki/domena.png?raw=true" style="width: 50%; height: 50%" /></p>

<p align="center">
  <i>Rys. 6. Widok panelu informacyjnego domeny</i>
</p>

___

<p align="center">
<img src="https://github.com/97703/DockerLab3/blob/main/rysunki/domena2.png?raw=true" style="width: 80%; height: 80%" /></p>

<p align="center">
  <i>Rys. 7. Widok panelu konfiguracyjnego domeny</i>
</p>

___

Skonfigurowano rekordy `DNS` typu **« _A_ »** w celu poprawnego przekierowania zapytań do uruchomionego serwera.

<p align="center">
<img src="https://github.com/97703/DockerLab3/blob/main/rysunki/dns.png?raw=true" style="width: 80%; height: 80%" /></p>

<p align="center">
  <i>Rys. 8. Skonfigurowane rekordy DNS</i>
</p>

___

<br><br><br>
## 3. Definiowanie aplikacji z wieloma kontenerami
*Utworzenie pliku typu Docker Compose Registry*
<br><br><br>
W celu uproszczenia uruchamiania i zarządzania wieloma kontenerami postawiono na utworzenie pliku typu **« _Docker Compose_ »** zamiast kilku plików **« _Dockerfile_ »** i uruchamiania osobno konkretnych kontenerów.
<br>
Utworzono **« _docker-compose.yml_ »** zawierający jeden kontener **« _Helloworld_ »**, który pozwala na wyświetlenie podstawowej strony `HTML`.

```diff
services:
    #️⃣ dodaj usługę helloworld
    helloworld:
        container_name: helloworld #️⃣ pod nazwą kontenera
        image: crccheck/hello-world #️⃣ z obrazu helloworld
        ports: #️⃣ na portach
            - 80:8000 #️⃣ mapuj port 80 na port 8000
```
<p align="center">
  <i>Rys. 9. Pierwsza wersja docker-compose.yml</i>
</p>

Uruchomiono aplikację zdefiniowaną w pliku.

<p align="center">
<img src="https://github.com/97703/DockerLab3/blob/main/rysunki/uruchomienie_dockera.png?raw=true"  style="width: 50%; height: 50%" /></p>

<p align="center">
  <i>Rys. 10. Uruchomienie aplikacji Docker Compose</i>
</p>

___

Po wpisaniu adresu **« _dockerlab3.website_ »** w pasku adresowym przeglądarki została wyświetlona strona *Hello World*.
<p align="center">
<img src="https://github.com/97703/DockerLab3/blob/main/rysunki/hello.png?raw=true" style="width: 50%; height: 50%" /></p>
<p align="center">
  <i>Rys. 11. Widok strony dockerlab3.website</i>
</p>

___

<br><br><br>
## 4. Konfiguracja usług kontenerowych
*Utworzenie plików wymaganych do poprawnego działania usług zawartych w kontenerach*
<br><br><br>
Utworzone zostały pliki: konfiguracyjny usługi serwera webowego **« _nginx_ »** oraz plik strony internetowej **« _index.html_ »**.

```diff
events {
        worker_connections 1024; #️⃣ ustaw limit połączeń
}

http {
    server_tokens off; #️⃣ wyłącz wyświetlenia informacji o wersji serwera w nagłówkach HTTP
    charset utf-8; #️⃣ ustaw zestaw znaków na UTF-8

    #️⃣ definiuj zachowanie serwera
    server {
        listen 80 default_server; #️⃣ nasłuchuj na porcie 80

        server_name _; #️⃣ obsługuj wszystkie przychodzące żądania HTTP

        #️⃣ definiuj lokalizację dla wszystkich żądań
        location / { 
            root /var/www/html; #️⃣ ścieżka do katalogu z plikami strony
            index index.html; #️⃣ w przypadku żądania, zwróć plik 'index.html'
        }

        #️⃣ definiuj lokalizacje do obsługi żądań protokołu ACME
        location ~ /.well-known/acme-challenge/ {
            root /var/www/certbot; #️⃣ ścieżka do katalogu weryfikacyjnego
        }
    }
}
```

<br>

<p align="center">
  <i>Rys. 12. Podstawowa konfiguracja usługi Nginx</i>
</p>

```diff
services:
  helloworld:
     container_name: helloworld
     image: crccheck/hello-world

#️⃣ dodaj usługę nginx
  nginx:
     container_name: nginx
     restart: unless-stopped #️⃣ kontener będzie resetowany chyba że jest zatrzymany ręcznie
     image: nginx 
     ports:
      - 80:80
      - 443:443
     volumes: #️⃣ określ wolumeny – pliki konfiguracyjne
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf #️⃣ pobierz plik konfiguracyjny z lokalnego folderu nginx
      - ./certbot/conf:/etc/letsencrypt #️⃣ wymóg certbota
      - ./certbot/www:/var/www/certbot #️⃣ wymóg certbota
      - ./nginx/index.html:/var/www/html/index.html #️⃣ pobierz plik strony internetowej

#️⃣ dodaj usługę certbot
  certbot:
     image: certbot/certbot
     container_name: certbot
     volumes:
      - ./certbot/conf:/etc/letsencrypt #️⃣ wymóg certbota
      - ./certbot/www:/var/www/certbot #️⃣ wymóg certbota
     #️⃣ wykonaj polecenie certyfikujące
     #️⃣ --webroot – umieść plik weryfikacyjny w katalogu domeny (-w wskazuje katalog)
     #️⃣ --force-renewal – zmusza usługę certbot do odnowienia certyfikatu
     #️⃣ --email – flaga określająca adres e-mail używany do powiadomień
     #️⃣ -d – określa nazwę domeny, dla której żądany jest certyfikat SSL
     #️⃣ --agree-tos – zgoda na warunki korzystania z usługi Let's Encrypt
     command: certonly --webroot -w /var/www/certbot --force-renewal --email s97703@pollub.edu.pl -d dockerlab3.website --agree-tos
```
<p align="center">
  <i>Rys. 13. Modyfikacja aplikacji Docker Compose</i>
</p>

<br><br><br>
## 5. Podpisanie certyfikatu
*Uruchomienie skonfigurowanej aplikacji Docker Compose w celu podpisania certyfikatu dla wybranej domeny*
<br><br><br>
Uruchomiono aplikację `Docker Compose` i uzyskano certyfikat wraz z wygenerowanym kluczem.

<p align="center">
<img src="https://github.com/97703/DockerLab3/blob/main/rysunki/generowanie_cert.png?raw=true" style="width: 70%; height: 70%" /></p>
<p align="center">
  <i>Rys. 14. Otrzymanie certyfikatu Let's Encrypt</i>
</p>

___

Otrzymany certyfikat wraz z kluczem należy zainstalować.

<br><br><br>
## 6. Instalacja certyfikatu
*Instalacja certyfikatu Let's Encrypt na serwerze Nginx*
<br><br><br>
Na serwerze w kontenerze Nginx aplikacji Docker Compose skonfigurowano nasłuchiwanie **« _portu 443_ »** oraz uruchomiono protokoły **« _SSL_ »** i **« _HTTPS_ »**. Podpięto certyfikat oraz klucz prywatny. Przekierowano żądania `HTTP` na `HTTPS`. Protokół `HTTPS` połączono z nazwą domenową **« _dockerlab3.website_ »**.

```diff
events {
        worker_connections 1024; #️⃣ ustaw limit połączeń
}

http {
    server_tokens off; #️⃣ wyłącz wyświetlenia informacji o wersji serwera w nagłówkach HTTP
    charset utf-8; #️⃣ ustaw zestaw znaków na UTF-8

    #️⃣ definiuj zachowanie serwera
    server {
        listen 80 default_server; #️⃣ nasłuchuj na porcie 80

        server_name _; #️⃣ obsługuj wszystkie przychodzące żądania HTTP

        #️⃣ definiuj lokalizację dla wszystkich żądań
        location / { 
            root /var/www/html; #️⃣ ścieżka do katalogu z plikami strony
            index index.html; #️⃣ w przypadku żądania, zwróć plik 'index.html'
        }

        #️⃣ definiuj lokalizacje do obsługi żądań protokołu ACME
        location ~ /.well-known/acme-challenge/ {
            root /var/www/certbot; #️⃣ ścieżka do katalogu weryfikacyjnego
        }

        return 301 https://$host$request_uri; #️⃣ przekierowanie żądania HTTP na HTTPS
    }

    server {
        listen 443 ssl http2; #️⃣ nasłuchuj na porcie 443, włącz obsługę SSL oraz protokół HTTP/2

        ssl_certificate     /etc/letsencrypt/live/dockerlab3.website/fullchain.pem; #️⃣ określa ścieżkę do certyfikatu SSL
        ssl_certificate_key /etc/letsencrypt/live/dockerlab3.website/privkey.pem; #️⃣ określa ścieżkę do klucza prywatnego
        server_name dockerlab3.website; #️⃣ określa nazwę hosta serwera

        location / {
            root /var/www/html;
            index index.html;
        }

        location ~ /.well-known/acme-challenge/ {
            root /var/www/certbot;
        }
    }
}
```
<p align="center">
  <i>Rys. 15. Końcowa konfiguracja serwera Nginx</i>
</p>

Ponownie uruchomiono aplikację `Docker Compose` za pomocą:

      docker compose up -d

<br><br><br>
## 7. Weryfikacja działania
*Przeprowadzenie testu odpowiedzi serwera z poziomu przeglądarki internetowej*
<br><br><br>
W przeglądarce internetowej **« _Microsoft Edge_ »** (*kompilacja 123.0.2420.65*) w pasku adresowym wpisano adres `IPv4`, a następnie nazwę domenową **« _dockerlab3.website_ »**. Rezultaty przedstawiono poniżej.
<p align="center">
<img src="https://github.com/97703/DockerLab3/blob/main/rysunki/brak-cert.png?raw=true" style="width: 50%; height: 50%" /></p>
<p align="center">
  <i>Rys. 16. Brak certyfikatu uwierzytalniającego dla adresu serwera</i>
</p>

___

<br>
<p align="center">
<img src="https://github.com/97703/DockerLab3/blob/main/rysunki/cert-2.png?raw=true" style="width: 50%; height: 50%" /></p>
<p align="center">
  <i>Rys. 17. Obecność certyfikatu uwierzytalniającego dla domeny serwera</i>
</p>

___

<br>
<p align="center">
<img src="https://github.com/97703/DockerLab3/blob/main/rysunki/cert.png?raw=true" style="width: 80%; height: 80%" /></p>
<p align="center">
  <i>Rys. 18. Aktywny certyfikat uwierzytelniający dla nazwy domenowej dockerlab3.website</i>
</p>

___

Certyfikat został poprawnie wystawiony przez `Let's Encrypt` 6 kwietnia 2024 roku o godzinie 2:13:54 dla domeny `dockerlab3.website`.
<br><br><br>
## 8. Automatyzacja certyfikacji
*Zautomatyzowanie procesu certyfikacji domeny dockerlab3.website za pomocą usługi Crontab*
<br><br><br>
Certyfikat podpisany przez `Let's Encrypt` jest ważny przez 90 dni, dlatego kluczowe jest regularne jego odnawianie.
<br>
Aby to osiągnąć, dodano polecenie **« _certonly_ »** do konfiguracji usługi **« _Crontab_ »**, która umożliwia automatyczne wywoływanie poleceń.

      crontab -e

<p align="center">
<img src="https://github.com/97703/DockerLab3/blob/main/rysunki/cron.png?raw=true" style="width: 70%; height: 70%" /></p>
<p align="center">
  <i>Rys. 19. Plik usługi crontab z nową regułą</i>
</p>

___

Certyfikowanie za pomocą aplikacji `Docker` zaplanowano na pierwszy dzień miesiąca, co dwa miesiące o 6 rano. Zablokowano wyświetlanie i zapisywanie wiadomości z konsoli za pomocą przekierowania strumienia (*>/dev/null 2>&1*).

      0 6 1 */2 * /snap/bin/docker compose -f /home/pieczykolan2/docker-compose.yml up certbot >/dev/null 2>&1


<br><br><br>
## 9. Podsumowanie
*Wnioski*

Celem zadania było wygenerowanie i zainstalowanie certyfikatu podpisanego przez `Let's Encrypt` dla dowolnej domeny.
<br><br>
Zadanie zostało w pełni zrealizowane przy użyciu maszyny wirtualnej [Google Cloud](https://cloud.google.com/?hl=pl), usług dostawcy domen [home.pl](https://home.pl/) oraz platformy **« _Docker_ »**. Dodatkowo zastosowano praktyczne rozwiązanie polegające na uruchomieniu aplikacji `Dockera` za pomocą usługi `Crontab`, umożliwiającej wykonywanie określonych poleceń w ustalonym czasie.
<br><br>
Jak wynika z przeprowadzonego zadania, certyfikat jest ważny 90 dni i wydawany tylko dla określonej nazwy domenowej, a nie dla adresu serwera.
