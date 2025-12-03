# 🔐 Web Application Security Test Report

## 1️⃣ Introduction

**Tester(s):**  
- Name: Veikko  

**Purpose:**  
- Phase 1 – Docker & ZAP Security Testing  
- Tunnistaa kriittiset haavoittuvuudet rekisteröinti-, autentikointi- ja käyttöoikeusprosesseissa sekä arvioida sovelluksen suojaustaso.  

**Scope:**  
- Tested components: Käyttäjätietokanta, kirjautumis- ja rekisteröintilomakkeet, roolinhallinta, HTTP-pyynnöt, palvelimen tiedostopolut  
- Exclusions: - 
- Test approach: White box  

**Test environment & dates:**  
- Start: 23.11.2025 11:00  
- End: 23.11.2025 19:00  
- Environment details: Docker, Linux, MySQL, Chrome, OWASP ZAP  

**Assumptions & constraints:**  
- Käytössä testitunnukset  
- 3 skannausta  
- Ei tuotantoympäristön testaus  

---

## 2️⃣ Executive Summary

**Short summary:**  
Testauksessa havaittiin useita kriittisiä haavoittuvuuksia (SQL Injection, Path Traversal, puuttuvat CSRF-tokenit), jotka mahdollistavat tietomurrot ja käyttöoikeuksien väärinkäytön.  

**Overall risk level:** 🔴 **High**  

**Top 5 immediate actions:**  (HUOM: Hyödynnyetty tekoälyä! MS-Copilot)
1. Ota käyttöön salasanahashaus (bcrypt/argon2) käyttäjätietokannassa.  
2. Korjaa SQL Injection -haavoittuvuudet parametrisoiduilla kyselyillä.  
3. Estä Path Traversal -hyökkäykset validoimalla ja rajoittamalla tiedostopolkuja.  
4. Lisää CSRF-tokenit kaikkiin lomakkeisiin.  
5. Ota käyttöön Content Security Policy (CSP) XSS-hyökkäysten estämiseksi.  

---

## 3️⃣ Severity scale & definitions

| **Severity Level** | **Description**                                                                                                              | **Recommended Action**           |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------- | -------------------------------- |
| 🔴 **High**        | Vakava haavoittuvuus, joka voi johtaa täyteen järjestelmän kompromissiin tai tietomurtoon (esim. SQL Injection, Path Traversal). | *Immediate fix required*         |
| 🟠 **Medium**      | Merkittävä ongelma, joka vaatii erityisiä olosuhteita tai käyttäjän toimintaa (esim. CSRF, XSS).                              | *Fix ASAP*                       |
| 🟡 **Low**         | Pieni ongelma tai konfiguraatioheikkous (esim. heikko salasana).                                                              | *Fix soon*                       |
| 🔵 **Info**        | Ei suoraa riskiä, mutta hyödyllinen koventamiseen (esim. puuttuvat turvaheadereiden asetukset).                               | *Monitor and fix in maintenance* |

---

## 4️⃣ Findings

| ID   | Severity | Finding               | Description                                                   | Evidence / Proof |
|------|----------|-----------------------|---------------------------------------------------------------|------------------|
| F-01 | 🔴 High  | Salaamattomat salasanat | Käyttäjätietokanta tallentaa salasanat selkokielisinä.         | Kuva 1           |
| F-02 | 🔴 High  | SQL Injection          | Parametrien manipulointi (`AND 1=1 --`) palauttaa piilotettua dataa. | Kuva 3           |
| F-03 | 🔴 High  | Path Traversal         | URL-manipulaatio (`../`) mahdollistaa tiedostojen luvun palvelimelta. | Kuva 4           |
| F-04 | 🟠 Medium| Puuttuvat CSRF-tokenit | HTML-lomakkeissa ei ole anti-CSRF-suojausta.                  | Kuva 6           |
| F-05 | 🟠 Medium| Puuttuva CSP           | Sivulla ei ole Content Security Policy -asetuksia, altistaa XSS:lle. | Kuva 5           |
| F-06 | 🟡 Low   | Duplicate accounts     | Sama käyttäjä voidaan rekisteröidä useita kertoja.            | Kuva 7           |

<img width="1004" height="1370" alt="image" src="https://github.com/user-attachments/assets/7c123dbe-794a-440d-9aaa-0d6f8ae4f2f4" />
Kuva 1. Käyttäjä tietokanta näkyy salaamattomana  myös salasanat.

<img width="616" height="1045" alt="image" src="https://github.com/user-attachments/assets/48ed0085-9efb-40de-94d1-0c8d3c46763c" />
Kuva 2. ZAP skannauksella tehdyt haavoittuvuudet.

<img width="1004" height="450" alt="image" src="https://github.com/user-attachments/assets/ed7858ea-e96d-419f-84cd-ccdb5dd3d734" />
Kuva 3. SQL injektio on mahdollista.

<img width="1004" height="518" alt="image" src="https://github.com/user-attachments/assets/a4b6ec07-77d9-47f9-9d61-484a8ce3978b" />
Kuva 4. Path traversal attack on mahdollista sivulle.

<img width="627" height="169" alt="image" src="https://github.com/user-attachments/assets/fa6a9b3c-228d-41c1-871d-e68a2a4be8da" />
Kuva 5. Content Security Policyjä ei oltu asetettu sivulle.

<img width="627" height="80" alt="image" src="https://github.com/user-attachments/assets/0695b210-fc48-4d9f-bd38-9ff48bdb8577" />
Kuva 6. CSRF Tokenit puuttuu.

<img width="1004" height="615" alt="image" src="https://github.com/user-attachments/assets/07904039-8e30-4d3c-ab47-470dcd63e20a" />
Kuva 7. Antaa tehdä saman käyttäjän useamman kerran.


---
