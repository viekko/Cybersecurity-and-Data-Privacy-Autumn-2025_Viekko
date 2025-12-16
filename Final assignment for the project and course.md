# PortSwigger – Suoritetut labrat
*(Kuvakaappaus Dashboardista liitetään dokumenttiin **<span style="color:red">[KUVAVIITE]</span>**)*

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

---

# Booking System Project
Projekti toteutettiin **neljässä vaiheessa**, joista jokainen keskittyi eri osa-alueisiin web-sovelluksen tietoturvassa. Dokumentaatio löytyy GitHubista (linkki liitetään raporttiin).

---

## Phase 1 – Docker & ZAP Security Testing
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
- SQL Injection ja Path Traversal saatiin korjattua.  
- CSRF-tokenit ja CSP puuttuivat edelleen → altistaa XSS-hyökkäyksille.  
- Rekisteröintilomake salli alaikäisten käyttäjien luomisen.  

**Mitä opin:**  
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
| chaos123!      | Hashcat + dive.rule                 |
| iamvengeance   | Hashcat, rule-based                |

**Mitä toimi / ei toiminut:**  
- Yleiset salasanat murtuivat nopeasti.  
- Hashcatin sääntöpohjaiset hyökkäykset tehokkaita monimutkaisempia salasanoja vastaan.  
- Kaikkia hasheja ei saatu murrettua → vahvat, pitkät salasanat pitivät pintansa.  

**Mitä opin:**  
- Offline-hyökkäykset ovat tehokkaita, jos hashit vuotavat.  
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
- Jokainen endpoint on validoitava roolin mukaan, ei vain kriittiset toiminnot.  
- Selkeä virheenkäsittely on tärkeää, jotta haavoittuvuudet eivät jää piiloon.

---

## Phase 4 – GDPR & Privacy Compliance
**Tavoite:** Arvioida sovelluksen GDPR-yhteensopivuutta.

**Keskeiset löydökset:**  
- Henkilötiedot näkyivät API:ssa ilman riittävää rajoitusta.  
- Rekisteröidyn oikeuksia (tarkastus, oikaisu, poisto) ei toteutettu.  
- Privacy Policy, Cookie Policy ja Terms of Service puuttuivat.  

**Parannukset:**  
- Lisätty tarvittavat tietosuojadokumentit.  
- Aloitettu Privacy by Design -periaatteiden huomioiminen.  

**Mitä opin:**  
- GDPR ei ole vain lakitekstiä – se vaikuttaa suoraan sovelluksen rakenteeseen ja API-suunnitteluun.  
- Tietosuojan huomioiminen alusta asti säästää aikaa ja vähentää riskejä.

---

## 📝 Reflektio
Kurssi tarjosi kokonaisvaltaisen katsauksen web-sovellusten tietoturvaan.  
PortSwigger-labrat antoivat käytännön kokemusta SQL-injektioista, XSS-hyökkäyksistä ja käyttöoikeusongelmista.  
Booking System -projekti auttoi soveltamaan oppeja todellisessa sovelluksessa ja ymmärtämään, miten pienet virheet voivat johtaa vakaviin tietoturvariskeihin.  
Opin myös, kuinka tärkeää on yhdistää tekninen testaus, turvallinen koodaus ja tietosuojavaatimukset kokonaisuutena.
