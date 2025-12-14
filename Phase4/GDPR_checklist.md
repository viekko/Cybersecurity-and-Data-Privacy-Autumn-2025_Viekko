# GDPR-tarkistuslista – Varausjärjestelmä (Phase 4)

| GDPR-vaatimus | Täyttyy | Huomiot |
|--------------|--------|---------|
| **Lainmukaisuus, kohtuullisuus ja läpinäkyvyys** | ❌ Ei | Vaikka tietosuojaseloste on saatavilla, henkilötietoja (sähköpostiosoite ja user_token) paljastuu käyttöliittymässä ja API-rajapinnoissa ilman asianmukaista pääsynrajoitusta. Tämä rikkoo GDPR 5(1)(a). |
| **Käyttötarkoitussidonnaisuus** | ⚠️ Osittain | Henkilötietoja kerätään varausjärjestelmän toteuttamiseksi, mutta user_tokenien ja käyttäjätunnusten näkyminen API:ssa ylittää alkuperäisen käyttötarkoituksen. |
| **Tietojen minimointi** | ❌ Ei | Sähköpostiosoite, syntymäaika ja autentikointiin liittyvä user_token ovat nähtävissä tarpeettomasti. Varaajan identiteetti näkyy varausnäkymässä vastoin vaatimusta anonymiteetistä (GDPR 5(1)(c)). |
| **Tietojen paikkansapitävyys** | ⚠️ Osittain | Käyttäjä syöttää tiedot itse, mutta järjestelmä ei tarjoa keinoa tietojen tarkistamiseen tai korjaamiseen. |
| **Säilytyksen rajoittaminen** | ⚠️ Osittain | Tietoja säilytetään niin kauan kuin käyttäjätili on olemassa, mutta erillistä säilytysaikapolitiikkaa passiivisille tileille ei ole määritelty. |
| **Eheys ja luottamuksellisuus** | ❌ Ei | User_tokenit ja muut henkilötiedot näkyvät salaamattomina tietokannassa ja API-rajapinnassa. Vaikka salasanat on hashattu, tämä ei täytä GDPR 32 artiklan vaatimuksia. |
| **Käsittelyn oikeusperuste määritelty** | ⚠️ Osittain | Käsittely perustuu sopimukseen (GDPR 6 artikla 1(b)), mutta käyttäjille ei ole täysin läpinäkyvästi kerrottu kaikista käsittelytoimista, kuten tokenien tallennuksesta ja näkyvyydestä. |
| **Privacy by Design -periaate** | ❌ Ei | Järjestelmä ei oletusarvoisesti suojaa henkilötietoja, vaan altistaa ne näkyville API-rajapinnoissa ja käyttöliittymässä. Tämä rikkoo GDPR 25 artiklaa. |
| **Oikeus tarkastaa tiedot** | ❌ Ei | Käyttäjällä ei ole keinoa tarkastella kaikkia itseään koskevia tietoja järjestelmässä. |
| **Oikeus tietojen oikaisuun** | ❌ Ei | Käyttäjä ei voi itse muokata omia henkilötietojaan. |
| **Oikeus tietojen poistoon** | ⚠️ Osittain | Vain ylläpitäjä voi poistaa käyttäjän, eikä käyttäjällä ole itsenäistä oikeutta pyytää poistoa järjestelmän kautta. |
| **Oikeus saada tietoa** | ⚠️ Osittain | Tietosuojaseloste on saatavilla, mutta se ei kata kaikkia todellisia tietojen käsittelykäytäntöjä (esim. tokenit, API-näkyvyys). |
| **Suostumus vaaditaan** | ⚠️ Osittain | Käsittely perustuu sopimukseen, mutta evästeiden käyttöön ei ole erillistä suostumusmekanismia. |
| **Evästekäytäntö saatavilla** | ⚠️ Osittain | Käytössä on vain istuntoevästeitä, mutta evästeiden käytöstä ei ole toteutettu erillistä hyväksyntä- tai hallintamekanismia. |
| **Tietojen luovutus kolmansille osapuolille** | ✔️ Kyllä | Tietoja ei jaeta kolmansille osapuolille. |
| **Erityisiä henkilötietoryhmiä käsitellään** | ⚠️ Osittain | Syntymäaika ei ole GDPR 9 artiklan erityinen henkilötietoryhmä, mutta se on arkaluonteinen henkilötieto, erityisesti alaikäisten osalta, ja vaatii korotettua suojaa. |

## Yhteenveto

Varausjärjestelmä on vain osittain GDPR-yhteensopiva. Merkittävimmät puutteet liittyvät
henkilötietojen tarpeettomaan näkyvyyteen, autentikointiin liittyvien user_tokenien
suojaamattomaan käsittelyyn sekä Privacy by Design -periaatteen rikkomiseen.

Erityisen kriittinen havainto on se, että käyttäjien sähköpostiosoitteet, syntymäajat ja
user_tokenit ovat nähtävissä API-rajapintojen kautta ilman riittävää pääsynrajoitusta,
mikä muodostaa vakavan tietosuojariskin.


| GDPR-vaatimus | Täyttyy | Huomiot / Testaustulos |
|---------------|---------|-----------------------|
| Järjestelmä on käytettävissä verkkoselaimen kautta | ✅ Kyllä | Testattu selainkäytöllä, toimii kuten odotettu. |
| Käyttäjät voivat rekisteröityä ja kirjautua järjestelmään | ✅ Kyllä | Rekisteröinti ja kirjautuminen toimivat. |
| Rekisteröity ja kirjautunut käyttäjä toimii joko varaajana tai ylläpitäjänä | ✅ Kyllä | Käyttäjäroolit toimivat oikein. |
| Ylläpitäjä voi lisätä, poistaa ja muokata resursseja ja varauksia | ❌ Ei | Testauksen perusteella kokeilu ei onnistunut. (Kuva 2) |
| Ylläpitäjä voi poistaa varaajan | ❌ Ei | Testauksen perusteella ylläpitäjä ei pysty poistamaan käyttäjiä. (Kuva 2) |
| Varaaja voi varata resurssin, jos on vähintään 15-vuotias | ❌ Ei | Alle 15-vuotiaat eivät voi rekisteröityä palveluun, joten teknisesti rajoitus toteutuu, mutta ei varausprosessin kautta. (Kuva 4) |
| Resursseja voi varata tuntiperusteisesti | ✅ Kyllä | Varausjärjestelmä tukee tuntivarausta. |
| Varaustietojen näkyminen ilman kirjautumista ei paljasta varaajan henkilöllisyyttä | ✅ Kyllä | Julkinen näkymä näyttää vain varatut resurssit ilman henkilötietoja. (Kuva 3) |
| Asiakas vaatii järjestelmän GDPR-yhteensopivuutta | ⚠️ Osittain | Checklist tehty arviointia varten, mutta järjestelmässä havaittiin puutteita (user_token ja sähköpostin näkyvyys API:ssa). |
| Ohjelmisto on kehitetty Privacy by Design -periaatetta noudattaen | ⚠️ Osittain | Alun perin tyhjä, nyt luotu uusi sisältö privacypolicyn, terms of service ja cookiepolicyn kautta. Suojaus puutteellinen user_tokenien ja sähköpostien osalta. |



<img width="1919" height="173" alt="image" src="https://github.com/user-attachments/assets/1ddb63d7-0191-405e-b3ec-0849018b0afc" />

Kuva 1. Tietokannassa näkyy edelleen käyttäjänimi (sposti), syntymäaika, luontipäivämäärä, rooli ja user token salaamattomana. User password on hashattu.


<img width="447" height="349" alt="image" src="https://github.com/user-attachments/assets/0e87652a-f03f-4671-9809-5008d4fc34af" />

Kuva 2. Apin kautta näkyy myös raoittamasti käyttäjille toisten tietoja.


<img width="970" height="400" alt="image" src="https://github.com/user-attachments/assets/6eaacfe6-0ef7-4ede-8b54-337aa831c852" />

Kuva 3. UI perusteella varaustietojen näkyminen Guestille.


<img width="523" height="501" alt="image" src="https://github.com/user-attachments/assets/020a2aa3-f190-4d6a-8406-4624f77eb80a" />

Kuva 4. Register kohtaa kysyy käyttäjää hyväksymään palvelun käyttöehdot ennen rekisteröintiä.


<img width="598" height="405" alt="image" src="https://github.com/user-attachments/assets/72114aee-4dd2-485b-a1c5-f09f36baa20c" />

Kuva 5. Tyhjä Terms of Service. Eli käyttäjä ei tiedä todellisia ehtoja jos haluaa.


<img width="588" height="462" alt="image" src="https://github.com/user-attachments/assets/30b4698c-7783-4ef5-bdfb-4bb1c7ce0257" />

Kuva 6. Accountin tiedot. --> Ei voi poistaa sieltä käyttäjää edes vaikka kyseessä admin.


<img width="1516" height="1420" alt="image" src="https://github.com/user-attachments/assets/145fa151-880a-46bb-87f6-7b71d6cf04b9" />

Kuva 7. Tyhjä Cookiepolicy.


<img width="1822" height="1391" alt="image" src="https://github.com/user-attachments/assets/81fa5586-9a06-42a3-9a33-f4adc09fde76" />

Kuva 8. Tyhjä Privacy policy.


<img width="721" height="628" alt="image" src="https://github.com/user-attachments/assets/56aae02e-ff96-4bee-b980-11d74050d3a8" />

Kuva 9. Tuntiperusteinen varausjärjestelmä.


<img width="614" height="588" alt="image" src="https://github.com/user-attachments/assets/332aea63-927f-44ef-b4af-b436ea1f7e78" />

Kuva 10. Ei voi luoda nuorempaa kuin 15v käyttäjän.

