# Todo — Junction kanban (Meta: AI Glasses)

Kolme kaistaa, jokainen tekee **vain omia korttejaan** järjestyksessä ylhäältä alas. Omistajuus ja sopimukset (M1–M7): `TEAM.md`.
Sizes: S ≈ 15 min, M ≈ 1 h, L ≈ half a day. `P0` = demo dies without it, `P1` = judges notice, `P2` = only if idle.
Ajat suhteessa hackin alkuun (T+0). Dokumentaatio valmis; kaikki toteutuskortit aloittamatta. Tarkennetaan Meta-tuen vahvistamien kyvykkyyksien jälkeen. Virallinen deadline on vielä vahvistamatta.

---

## Ennen alkua (kaikki yhdessä)

| ID | Card | Size | Owner | Notes |
|---|---|---|---|---|
| PRE-1 | Metan pisteelle: README:n avoimet kysymykset (DAT-video, latenssi, ääni, laitteiden jako) | S | Murtsi | **Go/no-go**: jos jatkuvaa videota ei saa, harkitaan uudelleen |
| PRE-2 | Lasit lainaan + DAT-kehittäjätili / SDK asennettuna | S | Murtsi | |
| PRE-3 | Käydään `TEAM.md` läpi | S | kaikki | 30 min |
| PRE-4 | Yhteys näkövammaisjärjestöön tai paikan päällä oleviin testaajiin | S | Joonas | Pilottipolku + käyttäjätesti |

---

## Kaista A — Laite + reaaliaikaputki · Murtsi

| ID | Aika | Card | Pri | Size | Valmis kun |
|---|---|---|---|---|---|
| A-1 | T+0 → 1 | Sovellusrunko + `contracts` (M1, M2, M4) + toisto nauhoitetusta videosta | P0 | M | B ja C ajavat stubeja vasten |
| A-2 | T+1 → 4 | DAT-yhteys: luvat, istunto, ensimmäinen kuva laseista | P0 | L | Kuva näkyy puhelimessa |
| A-3 | T+4 → 6 | Nauhoitustila: virta + aikaleimat talteen (rata, eri valot) | P0 | M | B saa raakadataa merkittäväksi |
| A-4 | T+6 → 8 | Jatkuva virta → B:n malli; latenssimittaus `t_capture` → `t_received` | P0 | M | **Integraatio T+8** |
| A-5 | T+8 → 12 | Ulostulo: valmiit äänet laseihin, suunta, värinä, jono + keskeytys | P0 | M | **Integraatio T+12**: varoitus kuuluu |
| A-6 | T+12 → 16 | Laitteen tilat: lasit pois/päähän, kotelo, BT-katko + uudelleenyhdistys, akku, lupa evätty | P0 | L | Jokainen tila → `DeviceState` |
| A-7 | T+16 → 20 | Kuvanopeuden säätö (kävely vs. paikallaan), lämpö/akku mitattu | P1 | M | **Integraatio T+20** |
| A-8 | T+20 → 24 | Eleen/äänen syöte → `describe()` | P1 | M | |
| A-9 | T+24 → 30 | Debug-näkymä demoon: kuva + laatikot + latenssi (tuomareille, ei käyttäjälle) | P1 | M | |
| A-10 | T+30 → 34 | Ray-Ban Display: suuntanuoli heikkonäköisille (jos API sallii) | P2 | M | |
| A-11 | T+34 → 40 | Varavideo demosta, laitteiden lataus- ja hygieniarutiini | P0 | S | |

---

## Kaista B — Havainnointi + arviointi · Niko

| ID | Aika | Card | Pri | Size | Valmis kun |
|---|---|---|---|---|---|
| B-1 | T+0 → 1 | `Hazard`-stub (M3), `metrics.json`-stub (M6), merkintämuoto (M7) | P0 | S | |
| B-2 | T+1 → 6 | Valmis pieni tunnistusmalli puhelimeen (CoreML/TFLite), luokat kartoitettu `kind`-arvoihin | P0 | L | Toimii toistoa vasten < 80 ms |
| B-3 | T+6 → 10 | Merkintä: A:n nauhoitukset → 30+ klippiä, jako ajon mukaan (opetus / testi) | P0 | M | Testijoukkoa ei käytetä viritykseen |
| B-4 | T+10 → 12 | Seuranta (`track_id`) + karkea `proximity` + `approaching` | P0 | M | **Integraatio T+12** |
| B-5 | T+12 → 16 | Hienosäätö omille luokille: pylväs, ovi, portaat ylös/alas, pään korkeuden este | P0 | L | |
| B-6 | T+16 → 20 | `in_path` + pään korkeus; arviointi → ensimmäinen `metrics.json` (recall, latenssi) | P0 | M | **Integraatio T+20** |
| B-7 | T+20 → 26 | Monokulaarinen syvyys, jos parantaa lukuja; muuten pois ja kirjataan | P1 | M | |
| B-8 | T+26 → 32 | Virheanalyysi: hukatut esteet käsin läpi → `known_weaknesses` (pimeä, lasi, portaat alas) | P1 | L | |
| B-9 | T+32 → 42 | Uusi testiaineisto vieraasta paikasta, ajetaan ilman viritystä, luvut lukitaan T+42 | P0 | M | |

---

## Kaista C — Varoituspolitiikka + kuvailu + luottamus · Joonas

| ID | Aika | Card | Pri | Size | Valmis kun |
|---|---|---|---|---|---|
| C-1 | T+0 → 1 | `Alert`-stub (M4), `describe()`-stub (M5) | P0 | S | |
| C-2 | T+1 → 6 | Politiikka puhtaana funktiona: `in_path` + lähellä → varoitus, prioriteetit, vaimennus; testit B:n stub-listoilla | P0 | L | |
| C-3 | T+6 → 10 | Sanasto + äänet: lyhyet sanat, suunta, epävarmuuden ääni; "en näe nyt" `DeviceState`:sta | P0 | M | |
| C-4 | T+10 → 12 | Kytkentä A:n ulostuloon | P0 | S | **Integraatio T+12** |
| C-5 | T+12 → 18 | Väärät hälytykset / min B:n merkityillä klipeillä; kynnysten viritys opetusjoukolla | P0 | M | Luku `metrics.json`:iin B:n kautta |
| C-6 | T+18 → 24 | Kuvailu: kasvojen sumennus paikallisesti → näkömalli (esim. Gemini) → lyhyt teksti; offline-varapolku | P1 | L | |
| C-7 | T+24 → 30 | Saavutettavuuspalaute suostumuksella: vihjeiden ymmärrettävyys, säädöt, virheestä palautuminen; hallittu ja valvottu demo ilman unimaskia | P1 | L | **Integraatio T+28** |
| C-8 | T+30 → 36 | Onboarding äänellä: mitä tekee, mitä ei, hiljaisuus ≠ vapaa reitti | P0 | M | |
| C-9 | T+36 → 40 | Pilottipolku deckiin: kenen kanssa, mitä mitataan, tietosuoja | P1 | M | |

---

## Blocked

| ID | Blocked on | Owner |
|---|---|---|
| PRE-1 / A-2 | Actual Meta device/API access and verified stream/output/state capabilities | Murtsi |
| B-2 | Platform/runtime choice and representative consented or synthetic evaluation data | Niko |
| C-7 | Consented participant feedback plan; no participant or partner confirmed | Joonas |

## Challenge completion gates

- [ ] Functional first-person prototype and deck, not just a phone mirror.
- [ ] Camera permission denial/revocation, unavailable input, connectivity and device-state recovery shown.
- [ ] Performance trade-offs and live-glasses/replay measurements labelled honestly.
- [ ] Accessible onboarding, input, output and controls validated.
- [ ] Consent, local/cloud processing, recording retention and deletion documented.
- [ ] Plausible pilot hypothesis; no unconfirmed partnership claims.
- [ ] Controlled demo and backup video prepared; deadline and formats verified.
