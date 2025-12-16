# PortSwigger

## Suoritetut labrat

Alla on lista PortSwigger Web Security Academy -labratehtävistä, jotka suoritin tämän kurssin aikana. Kuvakaappaus Dashboard-näkymästä, joka näyttää suoritetut labrat, voidaan liittää erikseen tarvittaessa.

### SQL-injektiot

* SQL injection vulnerability in WHERE clause allowing retrieval of hidden data
* SQL injection vulnerability allowing login bypass
* SQL injection attack, querying the database type and version on Oracle

### Autentikointi ja salasanapohjainen kirjautuminen

* Username enumeration via different responses
* Username enumeration via subtly different responses
* 2FA simple bypass
* Password reset broken logic

### Käyttöoikeus- ja pääsynhallinnan haavoittuvuudet

* User role can be modified in user profile
* Unprotected admin functionality
* Unprotected admin functionality with unpredictable URL
* User role controlled by request parameter
* User ID controlled by request parameter
* User ID controlled by request parameter, with unpredictable user IDs
* User ID controlled by request parameter with data leakage in redirect
* User ID controlled by request parameter with password disclosure
* Insecure direct object references (IDOR)

### Cross-Site Scripting (XSS)

* Reflected XSS into HTML context with nothing encoded
* Stored XSS into HTML context with nothing encoded
* DOM XSS in document.write sink using source location.search
* DOM XSS in innerHTML sink using source location.search

Nämä labrat kattoivat laajasti yleisimpiä web-sovellusten haavoittuvuuksia ja antoivat käytännön kokemusta niiden hyödyntämisestä realistisissa ympäristöissä.

---

# Booking system -projekti

Projekti jaettiin useaan vaiheeseen (Phase 1–4), jotka on dokumentoitu GitHub-repositoriossani:
[https://github.com/viekko/Cybersecurity-and-Data-Privacy-Autumn-2025_Viekko](https://github.com/viekko/Cybersecurity-and-Data-Privacy-Autumn-2025_Viekko)

## Phase 1 – Docker & ZAP Security Testing

Phase 1 -vaiheessa keskityin booking system -sovelluksen tietoturvan testaamiseen Docker-pohjaisessa testausympäristössä hyödyntäen OWASP ZAP -työkalua. Testauksen tavoitteena oli tunnistaa kriittiset haavoittuvuudet erityisesti rekisteröinti-, autentikointi- ja käyttöoikeusmekanismeissa. Testaus toteutettiin white box -lähestymistavalla, jossa sovelluksen toiminta ja rakenne olivat ennalta tuttuja.

Mikä toimi:
ZAP-skannaus ja manuaalinen testaus paljastivat tehokkaasti vakavia haavoittuvuuksia, kuten SQL Injectionin, Path Traversal -ongelman sekä sen, että salasanat oli alun perin tallennettu selkokielisinä. Näiden ongelmien korjaaminen onnistui, ja korjaukset pystyttiin myös verifioimaan uudelleentestaamalla.

Mikä ei toiminut:
Kaikkia havaittuja haavoittuvuuksia ei saatu korjattua tässä vaiheessa. Anti-CSRF-tokenit puuttuivat edelleen lomakkeista, ja Content Security Policy (CSP) jäi osittain tai kokonaan toteuttamatta aikataulusyistä.

Eniten aikaa vei:
Haavoittuvuuksien syiden ymmärtäminen ja niiden yhdistäminen aiemmin PortSwigger-labratehtävissä opittuihin käsitteisiin vei eniten aikaa, erityisesti SQL Injectionin ja Path Traversal -ongelmien osalta.

Mitä opin:
Opin, kuinka helposti vakavat tietoturvaongelmat voivat syntyä, jos turvallisuus jätetään huomiotta kehityksen alkuvaiheessa. Lisäksi ymmärsin paremmin, miten automaattiset työkalut kuten OWASP ZAP tukevat, mutta eivät korvaa, manuaalista testausta ja turvallista koodauskäytäntöä.

## Phase 2 – Password Cracking & Hash Analysis

Phase 2 -vaiheessa keskityin salasanojen murtamiseen ja hashattujen salasanojen analysointiin käyttäen Kali Linux -työkaluja Docker-ympäristössä. Testauksen tavoitteena oli ymmärtää, kuinka heikosti valitut salasanat voidaan murtaa offline-hyökkäyksillä, jos hyökkääjä saa haltuunsa järjestelmän tietokannan tai salasana-hashit.

Testauksessa hyödynsin John the Ripper- ja Hashcat-työkaluja, ja kohteena olivat MD5-hashatut salasanat. Käytin sekä valmiita sanalistoja (rockyou.txt), omia sanalistoja että sääntöpohjaisia hyökkäyksiä (rule-based attacks).

Murretut salasanat

Seuraavat salasanat saatiin onnistuneesti murrettua:

carrots123 – murrettu John the Ripperillä käyttäen raw-MD5-formaattia

donuts4life – murrettu John the Ripper dictionary attack -menetelmällä

darkside42 – murrettu John the Ripper dictionary attack -menetelmällä

iamironman – murrettu John the Ripper dictionary attack -menetelmällä

chaos123! – murrettu Hashcatilla käyttäen rockyou.txt-sanalistaa ja dive.rule-sääntöjä

iamvengeance – murrettu Hashcatilla rule-based attack -menetelmällä

(Katso kuvat 2–4.)

Käytetyt menetelmät

Dictionary attack: Hyödynsi yleisiä salasanalistoja ja paljasti useita helposti arvattavia salasanoja.

Rule-based attack: Sääntöjen avulla generoitiin variaatioita (numerot, erikoismerkit, kirjainkoot), mikä mahdollisti monimutkaisempien salasanojen murtamisen.

Optimized kernel (-O): Paransi suorituskykyä, vaikka ei yksinään lisännyt murrettujen salasanojen määrää.

Mask attack (teoreettinen): Tarkasteltiin maskihyökkäystä tehokkaana vaihtoehtona tilanteissa, joissa salasanamallit ovat ennakoitavia (esim. Word123!).

Mikä toimi

Yleiset ja ennakoitavat salasanat murtuivat nopeasti, erityisesti silloin kun ne perustuivat sanakirjasanoihin tai yksinkertaisiin variaatioihin. Hashcatin sääntöpohjaiset hyökkäykset osoittautuivat erityisen tehokkaiksi monimutkaisempien salasanojen kohdalla.

Mikä ei toiminut

Kaikkia hasheja ei saatu murrettua, vaikka käytössä oli useita sanalistoja, omia wordlistejä ja eri sääntökokoelmia. Tämä osoitti, että pidemmät ja vähemmän ennakoitavat salasanat kestävät merkittävästi paremmin offline-hyökkäyksiä.

Eniten aikaa vei

Komentojen optimointi, rule-tiedostojen polkujen korjaaminen sekä hyökkäysmenetelmien testaaminen ja vertaileminen veivät eniten aikaa.

Mitä opin

Opin konkreettisesti, kuinka vaarallista on käyttää heikkoja tai yleisiä salasanoja sekä vanhentuneita hash-algoritmeja kuten MD5:tä. Lisäksi ymmärsin, miksi hyökkääjälle pääsy salasana-hasheihin on erittäin vakava tietoturvariski ja miksi modernit hash-algoritmit (bcrypt, Argon2) ja pitkät salasanat ovat välttämättömiä.


## Vaihe 3 – Tietoturva ja parannukset

Kolmannessa vaiheessa keskityin järjestelmän parantamiseen ja tietoturvan huomioimiseen erityisesti PortSwigger-labratehtävistä opittujen asioiden pohjalta.

**Mikä toimi:** Tietoturva-ajattelun soveltaminen paransi järjestelmän luotettavuutta.

**Mikä ei toiminut:** Osa tietoturvaratkaisuista oli vaikeampi toteuttaa kuin aluksi ajattelin.

**Eniten aikaa vei:** Teoreettisten tietoturvakäsitteiden yhdistäminen käytännön toteutukseen.

**Mitä opin:** Tietoturva on huomattavasti helpompi toteuttaa, kun se huomioidaan jo suunnitteluvaiheessa.

## Vaihe 4 – Viimeistely ja dokumentointi

Viimeisessä vaiheessa siivosin koodia, testasin järjestelmää ja dokumentoin projektin GitHubiin.

**Mikä toimi:** Dokumentointi auttoi selkeyttämään, mitä projektissa tehtiin ja miksi.

**Mikä ei toiminut:** Ajanhallinta projektin loppuvaiheessa oli haastavaa.

**Eniten aikaa vei:** Selkeän dokumentaation kirjoittaminen ja aiemman työn läpikäynti.

**Mitä opin:** Hyvä dokumentaatio on olennainen osa teknistä projektia.

---

## Reflektio (50–100 sanaa)

Tämän aiheen aikana opin paljon todellisista web-sovellusten tietoturvahaavoittuvuuksista ja siitä, kuinka pienetkin virheet voivat johtaa vakaviin tietoturvaongelmiin. PortSwigger-labrat tarjosivat käytännön kokemusta SQL-injektioista, autentikointivirheistä, käyttöoikeusongelmista ja XSS-haavoittuvuuksista. Booking system -projekti auttoi soveltamaan näitä oppeja käytännössä. Ymmärrän nyt paremmin hyökkääjän ajattelutapaa sekä sen, miksi turvallinen koodaus ja testaus ovat keskeisiä web-kehityksessä.
