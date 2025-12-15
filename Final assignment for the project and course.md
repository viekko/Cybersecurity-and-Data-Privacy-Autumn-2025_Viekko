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

## Vaihe 1 – Suunnittelu ja alustus

Ensimmäisessä vaiheessa keskityin booking system -järjestelmän vaatimusten ymmärtämiseen ja kehitysympäristön pystyttämiseen. Tähän kuului järjestelmän rakenteeseen tutustuminen sekä päätösten tekeminen käyttäjistä, kirjautumisesta ja varaustoiminnallisuuksista.

**Mikä toimi:** Alustaminen sujui hyvin ja auttoi hahmottamaan projektin laajuutta.

**Mikä ei toiminut:** Osa suunnitteluratkaisuista jouduttiin muuttamaan myöhemmissä vaiheissa.

**Eniten aikaa vei:** Järjestelmän eri osien välisen logiikan ymmärtäminen.

**Mitä opin:** Huolellinen suunnittelu on tärkeää, mutta toteutuksen aikana ratkaisut usein kehittyvät.

## Vaihe 2 – Ydintoiminnallisuudet

Tässä vaiheessa toteutin booking systemin keskeiset toiminnallisuudet, kuten käyttäjätoiminnot, varausten käsittelyn ja perusdatankäsittelyn.

**Mikä toimi:** Perustoiminnot saatiin toimimaan ja järjestelmästä tuli käyttökelpoinen.

**Mikä ei toiminut:** Osa logiikasta sisälsi virheitä, jotka vaativat korjausta ja uudelleentoteutusta.

**Eniten aikaa vei:** Virheiden etsintä ja eri käyttäjäpolkujen testaaminen.

**Mitä opin:** Pienetkin logiikkavirheet voivat aiheuttaa merkittäviä ongelmia ilman riittävää testausta.

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
