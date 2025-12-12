Authorization & Access Control Test Report

Application: Reservation System
Tester:
Date:

1. Overview

Tämä raportti sisältää kaikki tekemäsi manuaaliset testit: Guest, Reserver ja Administrator -tasot. Mukana on mitkä endpointit toimivat, mitkä eivät, ja millaisia poikkeamia tai haavoittuvuuksia löytyi.

Kaikki löydökset perustuvat konkreettisiin URL-testauksiin, DevTools-yrityksiin sekä API-reittien manuaalisiin kokeiluihin.

2. Summary of Findings

🔴 Vakavat löydökset:

Guest ja Reserver näkevät /api/users → GDPR-riski.

Reserver voi nähdä osittain muiden varausten tietoja (IDOR-vaara, riippuen ID:stä palautui välillä tietoa, välillä Not found).

Admin-paneelin URL:t eivät avaudu edes adminille → käyttöliittymä-/reititysbugi.

🟡 Keskitaso:

Useat UI-sivut palauttavat Not Found sen sijaan että ohjaisivat kirjautumaan.

API palauttaa välillä tyhjiä listoja, välillä tietoja ilman selkeää logiikkaa (roolirajoitusten epäjohdonmukaisuus).

🟢 OK-asioita:

Guest ei pääse varausnäkymiin.

Reserver ei pääse admin-API-endpointeihin.

3. OSA A – Guest (ei kirjautunut)
3.1 Testatut sivut
URL	Tulokset	Arvio
/	OK	Julkinen
/login	OK	Julkinen
/register	OK	Julkinen
/profile	❌ Not Found	Ei pitäisi olla 404 vaan 401/redirect
/reservation	❌ Unauthorized	OK
/resources	OK	Julkinen, palautti listan/tyhjän
/admin	❌ Not Found	OK estetty (mutta väärä koodi)
/admin/*	❌ Not Found	OK estetty
3.2 API-endpointit
Endpoint	Tulos	Pitäisikö?	Kommentti
/api/users	❌ Näkyvissä	❌ Ei pitäisi	GDPR-riski
/api/users/1	OK Not Found	✔️	Hyvä
/api/resources	OK ([])	✔️	Julkinen ok
/api/resources/1	❌ Unauthorized	✔️	Ok
/api/reservations	❌ []	✔️ Ei pitäisi nähdä mitään	Ok (palautti tyhjän)
/api/reservations/1	OK Not found	✔️	Ok
4. OSA B – Reserver
4.1 UI-sivut
URL	Tulos
/profile	❌ Not Found
/reservation	OK
/reservation/new	❌ Not Found
/resources	OK
/admin/*	❌ Not Found (estetty)
4.2 API-endpointit
Endpoint	Tulos	Riskitaso
/api/reservations	Palautti kaikkien varaukset	🔴 IDOR / tietovuoto
/api/reservations/1	Joskus Not found, joskus palautti toisen käyttäjän varausta	🔴 IDOR
/api/admin/users	Estetty	✔️
/api/admin/resources	Estetty	✔️
/api/resources	OK	✔️
/api/users	Palauttaa kaikki käyttäjät	🔴 GDPR-riski
4.3 Roolien väärinkäyttötestit (POST/PUT/DELETE)

DevTools-yrityksissä et saanut POST/PUT/DELETE onnistumaan → Hyvä merkki.

Backend estää suurimman osan, mutta testaus oli vaikeaa koska pyyntöjä ei ollut UI:ssa.

Silti roolirajoitukset ovat puutteelliset GET-pyynnöissä.

5. OSA C – Administrator
5.1 UI

Kaikki admin-näkymät palauttivat: “The process failed — Not Found”

→ 🔴 Admin-paneeli ei ole käytettävissä reittitasolla.

5.2 API

Admin pystyi hakemaan:

kaikki käyttäjät

kaikki resurssit

kaikki varaukset

Mutta useat POST/PUT/DELETE endpointit puuttuvat tai palauttavat “Not Found” → alustamaton API.

6. Risk Assessment
Risk	Kuvaus	Taso
GDPR / tietovuoto	/api/users näkyy Guestille ja Reserverille	🔴 Kriittinen
IDOR (Insecure Direct Object Reference)	Reserver voi nähdä/muokata muiden tietoja	🔴 Kriittinen
Admin UI rikki	Admin ei pääse admin-sivuille	🟡 Keskitaso
HTTP-statuskoodit väärin	Not Found vs Unauthorized	🟡 Keskitaso
API epäjohdonmukaisuus	osin tyhjiä listoja, osin dataa	🟡 Keskitaso
7. Suositukset

Lukitse /api/users kokonaan adminille.

Korjaa roolitarkistukset reservation- ja user-endpointeissa.

Tee 401/403 vastaus Not Foundin sijaan.

Korjaa Admin UI – reitit puuttuvat tai väärin konfiguroitu.

Lisää yhtenäinen autorisointikerros kaikkiin endpointteihin.

8. Liitteet / Screenshots

(Sijoita kuvakaappaukset tähän, jos haluat.)

9. Status

Kokonaisarvio:❗ Turvallisuuspuutteita, joista osa kriittisiä.
