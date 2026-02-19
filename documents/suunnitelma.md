# Projektin yleiskuvaus
## Game House - High Score Hall of Fame on järjestelmä, joka näyttää 15–20 retropelin ennätykset HDMI-näytöllä ja mahdollistaa niiden reaaliaikaisen päivittämisen mobiilisovelluksella.

## Valittu teknologiapino (Tech Stack)
- Mobiilisovellus: React Native + TypeScript (varmistaa tyypityksen ja vakaan kehityksen).

- Tietokanta: Firebase Realtime Database (Spark-ilmaisversio). Valittu reaaliaikaisuuden ja ilmaisuuden vuoksi.

- Näyttölaite: Raspberry Pi (käytössä oleva laite) + HDMI-näyttö.

- Näyttöliittymä: Web-pohjainen (HTML/CSS/JS) Chromium-selaimessa Kiosk-moodissa.

- Etähallinta: Python-pohjainen taustaprosessi Raspberryn turvallista sammuttamista varten mobiilisovelluksesta.

## Näyttöliittymän arkkitehtuuri (The Rotating Billboard)
- Koska pelejä on noin 20, staattinen lista on liian pieni luettavaksi. Valittu ratkaisu on dynaaminen karuselli:

- Näkymä: 5 peliä kerrallaan näytöllä (yhteensä 4 sivua).

- Vaihto: Automaattinen vaihto 15 sekunnin välein.

- Erikoisominaisuus: "NEW HIGH SCORE" -interupt. Kun tietokantaan tulee uusi tulos, karuselli pysähtyy ja näyttää koko ruudun animaation uudesta ennätyksestä.

- Retro-estetiikka: CRT-filtterit (scanlines), neon-hehku (text-shadow) ja 'Press Start 2P' -pikselifontti.


## Operatiiviset toiminnot
- Turvallinen sammutus: Mobiilisovelluksessa on virtapainike. Se muuttaa Firebasen commands/shutdown-arvon. Raspberryn Python-skripti havaitsee muutoksen ja ajaa sudo shutdown -h now.

- Varmuuskopiointi: Käytetään Read-Only File Systemiä (OverlayFS) muistikortin suojaamiseksi ja Git-repositoriota koodin hallintaan.


## Laitteistovaihtoehtojen vertailu ja valinta

Alla on yhteenveto harkituista vaihtoehdoista ja perusteet lopulliselle valinnalle.

| Vaihtoehto | Arvio ja huomioita | Päätös |
| :--- | :--- | :--- |
| **Raspberry Pi** | Pieni, muokattava ja tukee GPIO-pinnejä. Soveltuu täydellisesti kiosk-käyttöön. | **VALITTU** (Löytyi valmiina varastosta). |
| **Mini-PC** | Erittäin vakaa, sisältää SSD-levyn (ei SD-kortin hajoamista). Helppo konfiguroida. | **VARAVAIHTOEHTO**, jos RPi:n ei olisi ollut valmiina, tämä olisi ollut ehkä paras vaihtoehto. |
| **Firebase (Pilvi)** | **Plussat:** Reaaliaikainen, toimii mistä vain, helppo skaalata. **Miinukset:** Vaatii nettiyhteyden. | **VALITTU** (Ammattimaisin ja vakain ratkaisu). |
| **Chromecast** | Edullinen, mutta vaatii jatkuvan "casting"-yhteyden puhelimesta. | **HYLÄTTY** (Yhteyden katkeamisriski ja käyttömukavuuden puute). |
| **SSH-hallinta** | Suora yhteys ilman pilvipalveluita. Vaatii saman Wi-Fi-verkon, IP-osoitteen säätämistä ja on monimutkaisempi toteuttaa mobiilisovelluksessa. | **HYLÄTTY** (Valittu Firebase paremman käytettävyyden vuoksi sekä mahdollisen projektin monistuksen takia). |
