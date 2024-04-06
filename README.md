<img src="https://lh3.googleusercontent.com/fife/ALs6j_EknhAoztzaT4dOw1lnWe9FEW1Y0_u2a3k0pwtWkuSPtGf5fG9dGUsO4n1pC-xVuxk9oIoNAnR9D1R7mrF5eVjS853czCXb2S502KOCzJWTN0KVU7buI88AwjSjYlsEfWIdkQSwkgPLnTUJxMEkyzOLvLzqW2f64cCPFA7FlE1W4Ufbz9icdjoaV3IOr0uw-WszBEeqsHrXv6ec3LRLVxzDgmUBWkWXxF0fdZB72ST9beJ9wv2nnR5n_1R_s0dCNurKeXn9z9VNF9k0kmQWUghMRVm-XoP73EQMF-TCPtD0Nsann9Jjblvr-3sBimb8ujbmIJ3mhPsf68R_DgO-brZNxerMoLeJjdlG3N4UZwRRMoXGRbiKbC8LHCgaLoYfmrLs7iHEjc_QAzlfpV4Sj8frNgLgHgHVxIumrqV4fw0fnjq7LocLWM0ClskcE_e5g9Si6VdShqeC0ham-9tfszTSqWmZaN3zNb1Aj850aeH4M1HJEXpu8Cui7PTV7wX3eLl6XZgz5yHMWfJrbsxxtneIIUYPTRcRx2OGEjVRugzAhrUaV8ywlusR3gdACg53ZtFRyhDTZgT6GdXqVN2zXpvm0ZzA-OSnb1SLQHQperfRgu67mirkPHof4UOlEOkLSBr0QhMs3-jH5-K_6A4hIRGr_N7uBOlhBMeOGYs0VC_1LVPAbgeaKqvu-WMD_KR4wO9lKTIY_YEsKoBHv0rj9j02BJRBeTHMN-pWeVbMa92MgU8MwxpFdcQJ0pbWSYySAF3PzwRoL9ZwmqEv2G8ZcHRNHmFyxHQppyyyqE0Gdi1qhwrECmMCDUqSCUhIt4eTXDH8q04Km0KjemytdGd33XgFIuyV-y5Aa_hV6yZ2g-oHSryl3icU08MI7nieW-vvqXFSV5OPo8gSM-evTeQGGUxxtsH7Iv1biuVtUZCspCaN74sq_aUF_yaf_Jq0mC7ZotKZoFBuRRzZ-vfKyHPL05mdyT_LFP9Vb-2bHa80l3aLgTp8JDJf7WeFIOAyBK28NV3bW-26-Rq9qtROkwuX6kjCtWm01gzN_5y_evGfD-6aUhaw8oobwz_WGJ67PQmrR0UGOFGbR4rbYsYrSoVjMHQSK7PiNJJXNGd01QNp-0hnQydUDOTNSUzumjw=w2706-h1903-v0" height="100" />

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

#### Helloworld − kontener korzystający z obrazu **« _crccheck/hello-world_ »**, służący do wstępnego testowania funkcjonowania strony WWW.
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
<img src="https://public.sn.files.1drv.com/y4m97ckXFIkSuG8V3t-OMHKcPu_4puFwC46gxgOH2L9JzmpSGxvB5KtTMYwuRnI07BSDBayivGtG5Cvfyahlplh-a4zqH81qtjF8evPFQRizVork9buH5CVyhljW1o9FfgB7ExFG-PedhMG51Tfzve625rVZcVWmwewhpWHU9zKvRflGKYh2vo1RjW_waY78SIfzW7ro1TYVlisOgkrKi5fsjogeCzleXDHm9eVN-mC5Jc?AVOverride=1" height="75" /></p>

<p align="center">
  <i>Rys. 1. Utworzone konto prywatne</i>
</p>

___

Maszyna wirtualna została zamówiona i uruchomiona z dwoma wirtualnymi jednostkami CPU oraz `8 GB` pamięci RAM. Dysk rozruchowy został określony na stały o pojemności równej `30 GB`, a użytym systemem maszyny zostało `Ubuntu 20.04 LTS`.

<p align="center">
<img src="https://public.sn.files.1drv.com/y4mM866xEHNstgtVoVUZs-wSm31SgfpceAo7DmNVKJa2MrlB5z2V0z9OI8VoR_wlPW1CysGNl2yJMi5n22QsgqfTn8X1Ra9JRa5l1KkJL4jk0A2SM_EZAIVXs7Nf-PSUpeSmgaBO32iKSYUEXntofl5i3u9X4DbvgbYiC9SdfdKKwDvsTg4GZqJCTYh3-7aEBVBcPyAbVW9h3gziX8Us8wNXaTxv0QRufWKGVdrdWpyaqY?encodeFailures=1&width=1292&height=3286" height="700" /></p>

<p align="center">
  <i>Rys. 2. Ustalona specyfikacja maszyny wirtualnej</i>
</p>

___

<br>
<p align="center">
<img src="https://public.sn.files.1drv.com/y4mLdRYshaasQ9VclEPxWZrM_btKxvIOjAacL3tW2MQWEiU4dk52EwAuGX0kwgAmqX0NVK4yW0Te-UiZHYK05bosK54ys7Blgf1hBxRS7DBs56bD-HWk2bcxd6pjqNxlCCCENTITplCfjcO9dyNm4pBcSUBN_k1omOsdlJRwksoKUlPKc2x3937LuDKAVnmNqXlEYbHg9xhyfYGtMwzXyEGs_4eDzJBfrIeED78UUFqHog?encodeFailures=1&width=1292&height=3286" height="450" /></p>
<p align="center">
  <i>Rys. 3. Wybrany system oraz typ i pojemność dysku rozruchowego</i>
</p>

___

Z uwagi na fakt, że serwer zostanie uruchomiony w celu pełnienia funkcji hostingu strony internetowej, umożliwiono ruch zarówno protokołem `HTTP`, jak i `HTTPS`.

<p align="center">
<img src="https://public.sn.files.1drv.com/y4mpZ3t7qR_AjQDDJuhom-HEiuhIJ_pAFazU0nXTwOVrHkLdwrIvE8M2Qh2gwkrBfd8CkA83kJHkwfbkyfPLsb9Vtm3cpaURblxkoau2UKi-ikg_9kurd2Q8TzAhn11UUDOyBmRsi5JUGiitix0J4J9U9ser8YlOLXuka5bPkaTsHV8_dQAaczrNyG_TXIZccXdMNGNIgKVqRe5hrERenGwsJfe5F_VZ9PPevhjjjA2WwY?encodeFailures=1&width=1348&height=3286" height="100" /></p>
<p align="center">
  <i>Rys. 4. Ustawienia zapory sieciowej</i>
</p>

___

Maszyna wirtualna została utworzona i uruchomiona. Jej zewnętrznym adresem `IPv4` jest **« _34.116.193.145_ »**. [Google Cloud](https://cloud.google.com/?hl=pl) umożliwia połączenie zdalne z maszyną za pomocą protokołu `SSH`.

<p align="center">
<img src="https://public.sn.files.1drv.com/y4mhhDUYrJKMaHN0YidI7ke5gnuAtJ5vVoREiGVfe-1zrEFCj_dlwHTqM9BF2pdrHseERG7McXV9c3qPYiqqbwJPwhkv3V3kvnKfdy1ZPoI2nmX3z6ZmxWs3cQEqHhvl_3vHwSnNEBPUZ9y0hOdNMcPtLk-CtaTHytZhSq6KyCWvDsrxwyGa8Alet43bV73BUimC-s7Ve3eeK0h7XX7qYeMA64LsGJC6UuQNjL5pM-z1go?AVOverride=1" height="100" /></p>
<p align="center">
  <i>Rys. 5. Uruchomiona maszyna wirtualna docker-lab3 w serwisie Google Cloud</i>
</p>

___

<br><br><br>
### 2.2 Konfiguracja domeny w Home.pl

Wykupiono dzierżawę domeny na okres roku w serwisie [home.pl](https://home.pl/) za pomocą uczelnianego konta (*s97703@pollub.edu.pl*).

<p align="center">
<img src="https://public.sn.files.1drv.com/y4mVbnVGa896As9ttIa1UNCtSIIDbnt_627NlpRNICPMFEJVSuG-22KXaNbmu68vdE0lToqbQNKC38XzYBCsQLRK3SVP-f4rPxrKrFv0HZHZxU2yPAIAd2Plv0LQVXePM1JORWmKEbDwGp8u_jK9xXHYsDKaydfpeIDrDZytuGfG-XpYLtSfusZwOqUPkafjVJHX7kWdvPyZCxu_RnR-EXsVUYrBGIOzqpmFJDmHAiuUI0?AVOverride=1" height="200" /></p>

<p align="center">
  <i>Rys. 6. Widok panelu informacyjnego domeny</i>
</p>

___

<p align="center">
<img src="https://public.sn.files.1drv.com/y4m5xZmuhFJ4xAopRjL1M9bZp-J_fl5KfJoKcwSlEpqrZ8BLlEB7XeG8u_4cY91SDPxpkjhKR9R_3Z0r6osXY1Edo7ddURUdSR8dGTsJnR2QhzdTeY0Ik5W_vEIPtmF6GLt6-_7zCa2mAA5Q_NzWdA6xnlfIMFVVA_l_WMgKYwfrYAP1fcACe2mwx8GFpMPgBz4QJqfKYOOoXI7LAqNwLAUECkfGcExvEFKWupVjAjrc6k?AVOverride=1" height="250" /></p>

<p align="center">
  <i>Rys. 7. Widok panelu konfiguracyjnego domeny</i>
</p>

___

Skonfigurowano rekordy `DNS` typu **« _A_ »** w celu poprawnego przekierowania zapytań do uruchomionego serwera.

<p align="center">
<img src="https://public.sn.files.1drv.com/y4moNWzgieU5iVJngZrd0Hld9PQj0XDKqwlf9rhUSIOyTQAJGhO6g0uHB_PUueHLzLO3LVxk--ZCJnwWAP2w2SNOopdiPg-5PDbuT1W0kJMOuW1WMNowqki2pGPbBNKfUr0QEscsFT1sTlEKMCrKLAdjZ1eOdnjmRRA8pfE253ama5DG-YCXqc3k7YFlaH5KbQevDcenrf_SpZdSP5OHBzWbjACuloGTb8cg_HDcYnq4ao?AVOverride=1" height="200" /></p>

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
<img src="https://public.sn.files.1drv.com/y4mRsTxjvsA1VGlO2Wksl6idyOp7atXfHv7FlaI4iTjc3fr0-9PTLjmzCM0FRF8t4y3B_MD0KIMtagwEi7P-G6ntSXjI_2TkVlX5iJn3fdmV8q6uIIXacEF_OOVz2-dgkLYOVYVRA0wwrWYuzNz-zXEEdALh5_nLe692Czj84nnk7DKXMWh7Yfmt9CwE4YJ__diI65dmilAm1hIZphV-ZWkoUWT_q7KOUZRxI35QJrVyik?AVOverride=1" height="50" /></p>

<p align="center">
  <i>Rys. 10. Uruchomienie aplikacji Docker Compose</i>
</p>

___

Po wpisaniu adresu **« _dockerlab3.website_ »** w pasku adresowym przeglądarki została wyświetlona strona *Hello World*.
<p align="center">
<img src="https://public.sn.files.1drv.com/y4mONpploCewLb5Zk6pbj7CtZvcbzGC32OxMSK1RW1VuAgOdHNLVkDH3A359rnuvdTw4dDJmVbbKbZJAUDwQIH4xvyY2DLNgUJGJrwMda7z0f6lQ-W04KdFYQmK_goIGwIJxXDQAJLpzOST73jxjnMPV2Qpo0YAePuZoSygb2RzxW8v9rSZYCVigM0tKj7FGQR8BmWN13aOjlo1gjdBewA8ojGLzlbKx9fqsR_Ed_bnyhQ?AVOverride=1" height="255" /></p>
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
<img src="https://public.sn.files.1drv.com/y4mn1OP0qTjwYlVI3hxX4JxfFNGWe2vAwavWoRe-t_v1RgnvVJ7wfDSp5mML2jRSpEynFLnkM0Fdu8F4RWyov50wfuEUM2juIO1JoqRPpp2HdDAEaFRHRxRHkR9ZOZidAYnGskPxNo5Z95kkZkutg88tpc3oiGOYj07BpPRZHaB4kDIApdCOHTISYXixTSMbO8bJz3D0j7z11LcoNS5hyVWi980e7LMonLcsv0T3zgPcVI?AVOverride=1" height="500" /></p>
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

Ponownie uruchomiono aplikację 'Docker Compose' za pomocą:

      docker compose up -d

<br><br><br>
## 7. Weryfikacja działania
*Przeprowadzenie testu odpowiedzi serwera z poziomu przeglądarki internetowej*
<br><br><br>
W przeglądarce internetowej **« _Microsoft Edge_ »** (*kompilacja 123.0.2420.65*) w pasku adresowym wpisano adres `IPv4`, a następnie nazwę domenową **« _dockerlab3.website_ »**. Rezultaty przedstawiono poniżej.
<p align="center">
<img src="https://public.sn.files.1drv.com/y4mPMNldhUeSCe6TjSwTnW55M3cBKfxcNgDk-phukFFns63xZiFrErRj6WRuXrhPGU6HjhlIO6IwNLW278JVJ7dC-3ddfNR2UQIn9OQ7rnt5BmUnbZ_zD-9gIiL3JNw8werqwejpmyFFLdbA6QnNPNklrbTZbm9LnDHa3qTldxFPZwDlFgcKQtYSdqqRDXk9HGoiBtqCkeCw2ovyX3-VejME-Ka106DIIwuDuhIcYAIhZg?AVOverride=1" height="300" /></p>
<p align="center">
  <i>Rys. 16. Brak certyfikatu uwierzytalniającego dla adresu serwera</i>
</p>

___

<br>
<p align="center">
<img src="https://public.sn.files.1drv.com/y4mP7uGbY5PWfpYPK21Cm8dMMldClRqJUBxD-fHj00xQJhokzPfpbuAB-7EVtegU_ZCAuCJv0rMpSHMurhhoEoeXGwlkf5zqWEcWeJ3YVUFa32VNJg7XmEB3eGoVVzIEWQjjuJ7_tbjOV1Uqw9M3pSpM1M9ukBnCfmoCJHMw2x5zAEB0CulFkrhW2bIUSMm0-D1P54E6TOIvE8-LsEyKRLWGmlfZhX7ejmOj3OEZtlGuqI?AVOverride=1" height="250" /></p>
<p align="center">
  <i>Rys. 17. Obecność certyfikatu uwierzytalniającego dla domeny serwera</i>
</p>

___

<br>
<p align="center">
<img src="https://public.sn.files.1drv.com/y4mUo_B2He70lgomUKFBQkN-OFMsPuyP4uHgBnh2RGzICtT41SHNcFaIIDcXib8veipmmHTIf3YQQbPqA1GLVJ-E8j5LZkML9mZgkEg3EyRjE8vAAtMc8QhcBxOc3SzxGUgB14YVAHBwMKdgAFrPTZc0_SoKY2DNb--hlIE06FiiYTbBoC_ZJBfTLJFLkBot1SGQxYrykM44b00qPfWlF3IkE-n1ZdU_15HqFaBdxmZwBM?AVOverride=1" height="500" /></p>
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
<img src="https://public.sn.files.1drv.com/y4m42FAcgNDj-fGAqfiqViBFFLU6NdsxXEQ51HE6otOEfLnCLEc09zVfNDDP4Ca0QBJyUBP78Ej7rNG_HVh-v-HOhuBaxeVqer4QZonPKE1-kH8qorqPnsE-iChALFT1qqyUyDEiEfluhPZIyR3lGAYqfsk1lflQhPxcFL1eKdUICb4DQxZefSnNpftCHkzacriiQG3zS4p9Nxai12YDP656gy1EK3_uwLmJKCQjGTGzBY?AVOverride=1" height="300" /></p>
<p align="center">
  <i>Rys. 19. Plik usługi crontab z nową regułą</i>
</p>

___

Certyfikowanie za pomocą aplikacji 'Docker' zaplanowano na pierwszy dzień miesiąca, co dwa miesiące o 6 rano. Zablokowano wyświetlanie i zapisywanie wiadomości z konsoli za pomocą przekierowania strumienia (*>/dev/null 2>&1*).

      0 6 1 */2 * /snap/bin/docker compose -f /home/pieczykolan2/docker-compose.yml up certbot >/dev/null 2>&1


<br><br><br>
## 9. Podsumowanie
*Wnioski*

Celem zadania było wygenerowanie i zainstalowanie certyfikatu podpisanego przez `Let's Encrypt` dla dowolnej domeny.
<br><br>
Zadanie zostało w pełni zrealizowane przy użyciu maszyny wirtualnej [Google Cloud](https://cloud.google.com/?hl=pl), usług dostawcy domen [home.pl](https://home.pl/) oraz platformy **« _Docker_ »**. Dodatkowo zastosowano praktyczne rozwiązanie polegające na uruchomieniu aplikacji `Dockera` za pomocą usługi `Crontab`, umożliwiającej wykonywanie określonych poleceń w ustalonym czasie.
<br><br>
Jak wynika z przeprowadzonego zadania, certyfikat jest ważny 90 dni i wydawany tylko dla określonej nazwy domenowej, a nie dla adresu serwera.
