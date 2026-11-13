# Team — Meta: AI Glasses

> Planned responsibilities and M1–M7 contracts. No implementation or team acceptance of interface changes is implied by this documentation setup.

Kolme henkilöä, kolme kaistaa, **ei päällekkäisyyksiä**. Jokaisella on oma ydintekniikka ja oma mitattava tulos — kukaan ei tee "vain frontin ja pitchin".

## Kuka tekee mitä

Ehdotus, sama jako kuin tiimin Bouncer-projektissa (A alusta, B tunnistus + arviointi, C toinen kerros + selitykset). Vahvistetaan, jos haaste valitaan.

| | Kuka | GitHub | Rooli | Vastaa kysymykseen | Oma mitattava tulos |
|---|---|---|---|---|---|
| **A** | **Murtsi** | `@Murtsi` | Laite + reaaliaikaputki | *"Tuleeko kuva laseista ajoissa, ja toimiiko kaikki, kun jokin menee pieleen?"* | Latenssi p50/p95, kuvanopeus, tilasiirtymät testattu |
| **B** | **Niko** | `@leppiskerttu` | Havainnointi + arviointi | *"Mikä tuolla on, kuinka lähellä, ja onko se reitillä?"* | Recall reitin esteille, pään korkeuden esteet erikseen, `metrics.json` |
| **C** | **Joonas** | `@Joonaswaa` | Varoituspolitiikka + kuvailu + luottamus | *"Sanotaanko tästä jotain, miten, ja kuinka varmana?"* | Väärät hälytykset / min, käyttäjätestin tulos |

- **A (Murtsi)** rakentaa mobiilisovelluksen DAT:n päälle: luvat, kuvavirta, lasien tilat (päässä/pois, kotelo, akku, BT-katko), ääni- ja värinäulostulo, kuvanopeuden säätö. Tekee **nauhoitus- ja toistotilan**, jotta B ja C voivat kehittää ilman laseja. Integroi. Haasteen kriteerit nimeävät juuri nämä (luvut, virheet, yhteys, laitteen tila) — tämä on puoli teknisestä pisteytyksestä.
- **B (Niko)** tekee tunnistuksen: mallin valinta ja hienosäätö omille luokille (pylväs, ovi, portaat, pään korkeus), vienti puhelimeen, karkea syvyys, seuranta, "onko reitillä". Omistaa merkityn aineiston ja kaikki luvut (`metrics.json`).
- **C (Joonas)** tekee päätöksen siitä, mitä käyttäjä kuulee: priorisointi, vaimennus, epävarmuuden ilmaisu, "en näe" -tila, suuntaääni/värinäkuviot. Lisäksi pyydetty kuvailu (kasvojen sumennus → näkömalli) ja käyttäjätesti radalla. Mittaa väärät hälytykset per minuutti B:n tunnistusten päältä.

## Omistajuus

| Polku (ehdotus) | Omistaja |
|---|---|
| `glasses/app/` | A |
| `glasses/app/recorder/` | A |
| `glasses/contracts.*` | A (muutokset kolmen hengen sopimuksella) |
| `glasses/perception/`, `glasses/data/`, `glasses/metrics.json` | B |
| `glasses/policy/`, `glasses/describe/`, `glasses/usertest/` | C |
| `docs/PITCH.md`, deck | yhteinen T+38 jälkeen |

Säännöt: oma haara `a/…`, `b/…`, `c/…`; PR koskee vain omia polkuja.

## Sopimukset kaistojen välillä

Kaikki olemassa **stubeina T+1**, jotta kukaan ei odota ketään.

| # | Sopimus | Tuottaja → käyttäjä | Stub T+1 | Oikea versio |
|---|---|---|---|---|
| M1 | `Frame { t_capture, t_received, image, imu? }` | A → B | Toisto nauhoitetusta videosta | Elävä DAT-virta (T+8) |
| M2 | `DeviceState { worn, connected, battery, camera_ok, permission }` | A → C | Aina "ok" | Oikeat tapahtumat (T+12) |
| M3 | `Hazard { track_id, kind, bbox, head_level, proximity: near/mid/far, approaching, in_path, confidence }` | B → C | Käsin kirjoitettu lista per klippi | Malli (T+12), syvyys + reitti (T+20) |
| M4 | `Alert { priority, text_key, direction: left/center/right, haptic_pattern, interruptible }` | C → A | Yksi kiinteä ääni | Politiikka (T+12) |
| M5 | `describe(frame) -> str` (pyydettäessä) | C → A | Kiinteä teksti | Näkömalli + sumennus (T+24) |
| M6 | `metrics.json` (havainnointi + latenssi) | B → kaikki | `"stub": true` | T+20, lukittu T+42 |
| M7 | `labels/*.json` merkityille klipeille | B → C | 3 klippiä | 30+ klippiä (T+16) |

`Hazard` ei sisällä metrejä, jos B ei pysty mittaamaan niitä luotettavasti. C rakentaa politiikan sen varaan, mitä B oikeasti tietää.

## Integraatiopisteet

| Aika | Mitä kytketään | Valmis kun |
|---|---|---|
| **T+1** | Stubit M1–M7 | Kaikki ajavat oman koodinsa nauhoitusta vasten |
| **T+8** | Elävä kuvavirta laseista + B:n valmis malli | Kehyksiä tulee, laatikoita piirtyy debug-näkymään |
| **T+12** | Putki päästä päähän: lasit → tunnistus → politiikka → ääni | Ensimmäinen varoitus kuuluu laseista |
| **T+20** | Syvyys, reitti, tilat, `metrics.json` | Oikeat luvut, "en näe" toimii |
| **T+28** | Kuvailu + käyttäjätesti radalla | Demopolku toimii alusta loppuun |
| **T+40** | Feature freeze | Vain korjaukset, luvut, deck |

## Pitch

T+38 jälkeen jokainen tekee omat diansa: **A** demo + kestävyys (tilat, latenssi), **B** luvut ja rajoitukset, **C** käyttäjä, luottamus ja tietosuoja. Ongelma ja kohdeyleisö yhdessä.
