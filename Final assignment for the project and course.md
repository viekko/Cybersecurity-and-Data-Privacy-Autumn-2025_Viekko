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


## Phase 3 – Authorization & Access Control Testing

Phase 3 -vaiheessa keskityin booking / reservation system -sovelluksen käyttöoikeuksien ja roolipohjaisen pääsynhallinnan (authorization & access control) testaamiseen. Testaus tehtiin manuaalisesti eri käyttäjärooleilla: Guest (ei kirjautunut), Reserver ja Administrator. Tavoitteena oli selvittää, pääsevätkö käyttäjät vain niihin toimintoihin ja tietoihin, joihin heidän roolinsa perusteella tulisi olla oikeus.

Testaus perustui konkreettisiin URL-kokeiluihin, selaimen DevTools-työkaluihin sekä API-endpointtien manuaaliseen testaamiseen.

Mitä testattiin

UI-sivut (reititys ja näkymät)

REST API -endpointit (GET, POST, PUT, DELETE)

Roolien välinen eristys

Virheellisten HTTP-statuskoodien käyttö

IDOR- ja tietovuotoriskit

Keskeiset löydökset

Testauksessa havaittiin useita vakavia puutteita käyttöoikeusvalvonnassa:

Guest- ja Reserver-käyttäjät pystyivät näkemään /api/users-endpointin, mikä on merkittävä GDPR- ja tietovuotoriski.

Reserver-rooli pystyi näkemään muiden käyttäjien varauksia tietyissä tilanteissa (/api/reservations ja /api/reservations/{id}), mikä viittaa IDOR-haavoittuvuuteen.

Admin-käyttöliittymä ei ollut käytettävissä lainkaan, sillä admin-reitit palauttivat virheen “Not Found” jopa admin-käyttäjälle.

Useat sivut palauttivat 404 Not Found, vaikka oikeampi vastaus olisi ollut 401 Unauthorized tai 403 Forbidden, mikä vaikeuttaa virheiden tunnistamista ja heikentää sovelluksen selkeyttä.

Mikä toimi

Guest ei päässyt varausnäkymiin tai admin-endpointeihin.

Reserver ei päässyt admin-API-endpointeihin.

Backend esti suurimman osan luvattomista POST/PUT/DELETE-pyynnöistä, mikä on hyvä merkki perusroolisuojauksesta.

Mikä ei toiminut

GET-pyyntöjen roolirajoitukset olivat puutteellisia, mikä johti tietovuotoihin.

API-käyttäytyminen oli epäjohdonmukaista: välillä palautui tyhjiä listoja, välillä arkaluonteista dataa.

Admin UI:n puuttuminen esti täysipainoisen järjestelmänhallinnan testaamisen.

Eniten aikaa vei

Epäjohdonmaisen API-käyttäytymisen tulkinta sekä sen selvittäminen, johtuivatko ongelmat roolivirheistä, puuttuvista reiteistä vai keskeneräisestä toteutuksesta.

Mitä opin

Opin, että autorisointi on yksi web-sovellusten kriittisimmistä tietoturva-alueista, ja pienetkin virheet voivat johtaa vakaviin tietovuotoihin. Lisäksi ymmärsin, että käyttöoikeudet tulee validoida jokaisessa endpointissa, ei vain käyttöliittymän tasolla. Oikeiden HTTP-statuskoodien käyttö ja yhtenäinen autorisointikerros ovat keskeisiä turvallisen ja selkeän sovelluksen rakentamisessa.


## Phase 4 – GDPR & Privacy Compliance

Phase 4 -vaiheessa keskityin booking / reservation system -järjestelmän GDPR-yhteensopivuuden arviointiin ja tietosuojan parantamiseen. Työ toteutettiin GDPR-tarkistuslistan avulla, ja siinä analysoitiin järjestelmän henkilötietojen käsittelyä, näkyvyyttä, suojausta sekä rekisteröidyn oikeuksien toteutumista. Lisäksi täydensin järjestelmää luomalla Privacy Policy-, Cookie Policy- ja Terms of Service -sivut, jotka olivat alun perin puutteellisia tai tyhjiä.

Mitä testattiin ja arvioitiin

Henkilötietojen näkyvyys käyttöliittymässä ja API-rajapinnoissa

GDPR:n perusperiaatteet (artikla 5)

Käsittelyn oikeusperuste (artikla 6)

Tietoturva ja eheys (artikla 32)

Privacy by Design -periaate (artikla 25)

Rekisteröidyn oikeudet (artiklat 12–23)

Evästeiden ja tietosuojaselosteiden olemassaolo ja sisältö

Keskeiset löydökset

GDPR-tarkistuslistan perusteella järjestelmä on vain osittain GDPR-yhteensopiva. Merkittävimmät puutteet liittyvät henkilötietojen suojaamiseen ja näkyvyyteen:

Sähköpostiosoitteet, syntymäajat ja autentikointiin liittyvät user_tokenit näkyvät API-rajapinnoissa ilman riittävää pääsynrajoitusta, mikä muodostaa vakavan GDPR- ja tietovuotoriskin.

User_tokenit ja muut henkilötiedot ovat nähtävissä salaamattomina tietokannassa ja API:ssa, vaikka salasanat onkin hashattu.

Järjestelmä ei noudata Privacy by Design -periaatetta, sillä henkilötiedot ovat oletusarvoisesti näkyvissä.

Rekisteröidyn oikeudet (tietojen tarkastus, oikaisu ja poisto) eivät toteudu teknisesti, eikä edes ylläpitäjä pysty poistamaan käyttäjiä järjestelmästä.

Tietosuojaseloste, evästekäytäntö ja käyttöehdot olivat aluksi tyhjiä tai puutteellisia, jolloin käyttäjä ei saanut riittävästi tietoa henkilötietojen käsittelystä.

Mikä toimi

Järjestelmä kerää suhteellisen vähän henkilötietoja.

Julkinen varausnäkymä ei paljasta varaajan henkilöllisyyttä.

Alle 15-vuotiaat eivät voi rekisteröityä palveluun.

Resurssien tuntiperusteinen varaus toimii teknisesti oikein.

Tehdyt parannukset

Phase 4:n aikana loin ja lisäsin järjestelmään:

Privacy Policy – kuvaamaan henkilötietojen käsittelyä

Cookie Policy – dokumentoimaan istuntoevästeiden käytön

Terms of Service – määrittelemään palvelun käyttöehdot

Nämä lisäykset paransivat järjestelmän läpinäkyvyyttä ja vastasivat osittain GDPR:n tiedonantovelvoitteisiin, mutta tekniset suojauspuutteet jäivät edelleen ratkaisematta.

Mikä ei toiminut

Henkilötietojen näkyvyys API-rajapinnoissa ei ole riittävästi rajattu.

Käyttäjien tietojen poistaminen tai muokkaaminen ei ole mahdollista.

Evästeiden käyttöön ei ole toteutettu varsinaista suostumusmekanismia.

Admin-käyttöliittymä ei tue GDPR-vaatimuksia (esim. käyttäjien hallinta).

Eniten aikaa vei

GDPR-vaatimusten tulkitseminen käytännön tekniseen toteutukseen sekä sen arviointi, mitkä puutteet ovat lainsäädännöllisesti kriittisimpiä.

Mitä opin

Opin, että GDPR-yhteensopivuus ei ole vain dokumentaatiota, vaan se vaatii myös teknisiä ratkaisuja, kuten pääsynhallintaa, tietojen minimointia ja turvallista API-suunnittelua. Lisäksi ymmärsin, kuinka tärkeää on huomioida Privacy by Design jo järjestelmän alkuvaiheessa, sillä myöhemmät korjaukset ovat huomattavasti työläämpiä ja riskialttiimpia.

---

## Reflektio (50–100 sanaa)

Tämän aiheen aikana opin paljon todellisista web-sovellusten tietoturvahaavoittuvuuksista ja siitä, kuinka pienetkin virheet voivat johtaa vakaviin tietoturvaongelmiin. PortSwigger-labrat tarjosivat käytännön kokemusta SQL-injektioista, autentikointivirheistä, käyttöoikeusongelmista ja XSS-haavoittuvuuksista. Booking system -projekti auttoi soveltamaan näitä oppeja käytännössä. Ymmärrän nyt paremmin hyökkääjän ajattelutapaa sekä sen, miksi turvallinen koodaus ja testaus ovat keskeisiä web-kehityksessä.
