# PortSwigger – Suoritetut labrat

Alla on lista PortSwigger Web Security Academy ‑labratehtävistä, jotka suoritin kurssin aikana. Labrat kattoivat keskeiset web-haavoittuvuudet ja antoivat käytännön kokemusta hyökkäysten toiminnasta ja niiden estämisestä.

---

## 🛡 SQL Injection
- SQL injection vulnerability in WHERE clause allowing retrieval of hidden data  
- SQL injection vulnerability allowing login bypass  
- SQL injection attack, querying the database type and version on Oracle  

## 🔑 Authentication & Passwords
- Username enumeration via different responses  
- Username enumeration via subtly different responses  
- 2FA simple bypass  
- Password reset broken logic  

## 🔐 Access Control & Authorization
- User role can be modified in user profile  
- Unprotected admin functionality  
- Unprotected admin functionality with unpredictable URL  
- User role controlled by request parameter  
- User ID controlled by request parameter  
- User ID controlled by request parameter, with unpredictable user IDs  
- User ID controlled by request parameter with data leakage in redirect  
- User ID controlled by request parameter with password disclosure  
- Insecure direct object references (IDOR)  

## 💻 Cross-Site Scripting (XSS)
- Reflected XSS into HTML context with nothing encoded  
- Stored XSS into HTML context with nothing encoded  
- DOM XSS in document.write sink using source location.search  
- DOM XSS in innerHTML sink using source location.search  

<img width="715" height="372" alt="image" src="https://github.com/user-attachments/assets/ca4224f7-7f15-4f28-8389-cf85850aa501" />
<img width="588" height="321" alt="image" src="https://github.com/user-attachments/assets/cfe30062-d134-425c-8391-25da439eb4ae" />
<img width="887" height="1126" alt="image" src="https://github.com/user-attachments/assets/6b423424-4813-4466-8464-8328c01e6008" />
<img width="1004" height="576" alt="image" src="https://github.com/user-attachments/assets/030e33e5-3fbb-4a16-9123-3c2694df7fab" />

Kuvasarja 1. Suorittamani Portswigger labit-


---

# Booking System Project
Projekti toteutettiin **neljässä vaiheessa**, joista jokainen keskittyi eri osa-alueisiin web-sovelluksen tietoturvassa. Dokumentaatio löytyy GitHubista (linkki liitetään raporttiin).

---

## Phase 1 – Docker & ZAP Security Testing Part 1 & 2
**Tavoite:** Tunnistaa kriittiset haavoittuvuudet rekisteröinti-, autentikointi- ja käyttöoikeusprosesseissa.  

**Testiympäristö:** Docker, Linux, MySQL, Chrome, OWASP ZAP  
**Menetelmä:** White box  
**Aika:** 23.11.2025, klo 11:00–19:00  
**Scope:** Käyttäjätietokanta, kirjautuminen, rekisteröinti, roolinhallinta, HTTP-pyynnöt, palvelimen tiedostopolut  

**Keskeiset löydökset:**

| ID   | Severity | Finding                     | Status       |
|------|---------|-----------------------------|-------------|
| F‑01 | 🔴 High    | Salaamattomat salasanat     | Fixed ✅       |
| F‑02 | 🔴 High    | SQL Injection               | Fixed ✅       |
| F‑03 | 🔴 High    | Path Traversal              | Fixed ✅       |
| F‑04 | 🟠 Medium  | Puuttuvat CSRF‑tokenit      | Not fixed ⚠   |
| F‑05 | 🟠 Medium  | Puuttuva CSP                | Not fixed ⚠   |
| F‑06 | 🟡 Low     | Duplicate accounts          | Not critical   |

**Mitä toimi / ei toiminut:**  
- Tietokanta hashaus salasanoille korjattiin.
- SQL Injection ja Path Traversal saatiin korjattua.  
- CSRF-tokenit ja CSP puuttuivat edelleen → altistaa XSS-hyökkäyksille.  
- Rekisteröintilomake salli alaikäisten käyttäjien luomisen.
- Part 1 tietokanta kokonaan hashaamatta. Voi luoda useampia samoja käyttäjiä.

**Mitä opin:**  
- Tietokannan salaaminen aina tärkeää. Korostuu jos nettirajapinnassa suoraan kiinni (case Vastaamo).
- SQL injektio palauttaa piilotettua dataa.
- Pienet puutteet voivat avata merkittäviä hyökkäysvektoreita.  
- ZAP automatisoi peruslöydökset hyvin, mutta manuaalinen testaus on välttämätöntä.

---

## Phase 2 – Password Cracking & Hash Analysis
**Tavoite:** Testata salasanojen vahvuutta ja analysoida hashattuja salasanoja offline-hyökkäyksillä.

**Menetelmät:**  
- John the Ripper (raw-MD5, sanakirjahyökkäykset)  
- Hashcat (dictionary + rule-based attacks, dive.rule, best64.rule, optimized kernel)  
- Mask attack (ennakoidut salasanamallit)  

**Murretut salasanat:**

| Password       | Tool / Method                       |
|----------------|------------------------------------|
| carrots123     | John the Ripper, raw-MD5           |
| donuts4life    | John the Ripper, dictionary        |
| darkside42     | John the Ripper, dictionary        |
| iamironman     | John the Ripper, dictionary        |
| chaos123!      | Hashcat                            |
| iamvengeance   | Hashcat                            |

**Mitä toimi / ei toiminut:**  
- Hashcat hidas ja vaatii paljon laskentatehoa.
- rockyou.txt hyvä salasana dictionary, mutta tarvii olla muita.
- Yleiset salasanat murtuivat nopeasti.  
- Hashcatin sääntöpohjaiset hyökkäykset tehokkaita monimutkaisempia salasanoja vastaan.  
- Kaikkia hasheja ei saatu murrettua → ei jaksanut odottaa Hashcatin ruksuttamista. Odottelin toistaiseksi ja oli mennyt vasta 3%.

**Mitä opin:**  
- Offline-hyökkäykset ovat tehokkaita, jos hashit vuotavat. Pitää salata aina tietokannat ja kaikki sensitiiviset tiedot mielellään.
- MD5 on täysin riittämätön – modernit hashit kuten bcrypt/Argon2 ovat välttämättömiä.

---

## Phase 3 – Authorization & Access Control Testing
**Tavoite:** Testata roolipohjaista pääsynhallintaa (Guest, Reserver, Admin).

**Keskeiset löydökset:**  
- Guest ja Reserver näkivät arkaluonteista dataa API-endpointeista → puutteellinen access control.  
- Reserver pystyi näkemään muiden varauksia → IDOR-haavoittuvuus.  
- Admin UI ei ollut käytettävissä → hallintatoimintoja ei voitu testata.  
- HTTP-statuskoodit olivat epäjohdonmukaisia.  

**Mitä toimi / ei toiminut:**  
- Backend esti suurimman osan luvattomista POST/PUT/DELETE-pyynnöistä.  
- GET-pääsynhallinta oli puutteellinen ja aiheutti tietovuotoja.  

**Mitä opin:**  
- API kutsuilla saa hyvin esiin piilotettua dataa.
- Haavoittuvuuksien huomioiminen ohjelmissa todella kriittistä.  

---

## Phase 4 – GDPR & Privacy Compliance
**Tavoite:** Arvioida sovelluksen GDPR-yhteensopivuutta.

**Keskeiset löydökset:**  
- Henkilötiedot näkyivät API:ssa ilman riittävää rajoitusta.  
- Rekisteröidyn oikeuksia (tarkastus, oikaisu, poisto) ei toteutettu.  
- Privacy Policy, Cookie Policy ja Terms of Service puuttuivat.  

**Parannukset:**  
- Lisätty tarvittavat tietosuojadokumentit Privacy Policy, Cookie Policy ja Terms of Service).   

**Mitä opin:**  
- GDPR vaikuttaa suoraan sovelluksen rakenteeseen ja API-suunnitteluun. Sen noudattaminen on tärkeää.
- Tietosuojan huomioiminen on nykyään todella tärkää.

---

## 📝 Reflektio
Kurssi tarjosi kokonaisvaltaisen katsauksen web-sovellusten tietoturvaan.  
PortSwigger-labrat antoivat käytännön kokemusta SQL-injektioista, XSS-hyökkäyksistä ja käyttöoikeusongelmista.  
Booking System -projekti auttoi soveltamaan oppeja todellisessa sovelluksessa ja ymmärtämään, miten pienet virheet voivat johtaa vakaviin tietoturvariskeihin.  
Opin myös, kuinka tärkeää on yhdistää tekninen testaus, turvallinen koodaus ja tietosuojavaatimukset kokonaisuutena.
