# Sixth Sense

Junction 2026 -haaste **Meta — AI Glasses for Everyone, Everyday**.

> **Status: idea-taso.** Haastetta ei ole vielä valittu, eikä stackia ole päätetty. Tiimin toinen projekti (Bouncer, Hostinger) on omassa repossaan.

**Sixth Sense**: lasit huomaavat esteen kävelyreitillä ja kertovat siitä näkövammaiselle äänellä tai värinällä, ajoissa ja vain silloin, kun sillä on väliä. Erityisesti pään korkeudella olevat esteet, joita valkoinen keppi ei tavoita.

| Tiedosto | Sisältö |
|---|---|
| [IDEA.md](IDEA.md) | Ongelma, ratkaisu, demo, rajaus, riskit, mittarit |
| [ARCHITECTURE.md](ARCHITECTURE.md) | Putki lasit → puhelin → tunnistus → varoitus, latenssibudjetti |
| [TEAM.md](TEAM.md) | Kaistat A/B/C ja sopimukset M1–M7 |
| [TODO.md](TODO.md) | Kortit kaistoittain T+0 → palautus |
| [CHANGELOG.md](CHANGELOG.md) | Muutosloki |

## Haaste lyhyesti

Lähde: [hackjunction.app/participate/45/challenges/342](https://hackjunction.app/participate/45/challenges/342), luettu 2026-11-13.

- **Tehtävä:** paras arkinen käyttötapaus Metan AI-laseille, jossa *ensimmäisen persoonan näkökenttä* mahdollistaa uuden tai paremman vuorovaikutuksen. Ei pelkkää puhelimen ruudun peilausta.
- **Palautus:** toimiva prototyyppi **ja** deck. Ongelma ja kohdeyleisö selkeästi.
- **Kelpaa:** display- ja audio-first-kokemukset ovat tasavertaisia, samoin web-appit ja **Device Access Toolkit (DAT)** -mobiili-integraatiot.
- **Prototyypin pitää käsitellä:** luvat, virheet, suorituskyvyn kompromissit, yhteyskatkot ja laitteen tilan muutokset.
- **Pakolliset teemat:** saavutettavuus kaikille kyvyille, asianmukainen suostumus ja turvallinen datan käsittely demon laajuudessa; uskottava polku käyttöönottoon tai pilottiin.

### Arviointi (4 × 25 %)

| Kriteeri | Miten me vastaamme |
|---|---|
| Luovuus ja käyttötapaus | Pään korkeuden esteet: aukko, jota keppi ja opaskoira eivät täysin kata, ja jonka vain silmien korkeudella oleva kamera näkee |
| Tekninen toteutus | Paikallinen tunnistus puhelimessa < 500 ms, laitteen tilojen ja yhteyskatkojen hallinta näkyvästi demossa |
| Todellinen arvo ja toteutettavuus | Mitattu tarkkuus ja väärät hälytykset per minuutti omalla videoaineistolla; pilottipolku näkövammaisjärjestön kanssa |
| UX: syöte, saavutettavuus, luottamus | Hiljainen oletuksena, kertoo epävarmuuden, ei koskaan "reitti on vapaa"; kasvot sumennetaan, mitään ei tallenneta |

### Resurssit

- 10 × **Meta Ray-Ban Display** ja 10 × **RBM Gen 2** (Ray-Ban Meta) jaettuna joukkueiden kesken, lataus ja hygieniatarvikkeet.
- Kehittäjätuki Metan pisteellä. Yhteyshenkilö **Amanda Le** (DAT, Android). Mentorit: web-appit (Ming Lin, William La, Amy Lee, Odil Nikadambaev), DAT iOS (Dennis Punjabi).
- Palkinto: voittajatiimin jäsenille Ray-Ban Meta -lasit; parhaat kutsutaan Metan AI-foorumiin 18.11.2026.
- Muista hackerpackeista hyödyllisiä: **Gemini** (näkömalli kuvailuun pyydettäessä).

Haastesivu ei kerro DAT:n teknisiä rajoja. Alla olevat oletukset perustuvat DAT:n julkiseen esittelyyn, ja ne pitää varmistaa Metan pisteellä.

## Avoimet kysymykset Metan pisteelle

Kysy nämä ensimmäisenä tuntina. Vastaukset ratkaisevat, onko idea toteutettavissa 48 tunnissa.

**Laitteet ja jako**
- [ ] Montako laitetta yksi tiimi saa ja kuinka pitkäksi aikaa? Saako lasit pitää yön yli?
- [ ] Kumpi laite sopii tähän paremmin: RBM Gen 2 (kamera + ääni) vai Ray-Ban Display (myös näyttö + Neural Band)?

**Kamera (DAT)**
- [ ] Saako DAT:lla **jatkuvan videovirran** laseista puhelimeen, vai vain yksittäisiä kuvia?
- [ ] Resoluutio ja kuvanopeus virrassa? Pystyykö niitä säätämään?
- [ ] Mikä on **latenssi** lasien kamerasta puhelimen sovellukseen (ms)?
- [ ] Kuinka kauan virta kestää ennen kuin lasit kuumenevat tai akku loppuu?
- [ ] Mitä tapahtuu, kun lasit otetaan päästä, laitetaan koteloon tai Bluetooth katkeaa? Tuleeko tästä tapahtuma sovellukselle?

**Ääni ja syöte**
- [ ] Saako sovellus toistaa ääntä lasien kaiuttimista pienellä viiveellä? Toimiiko stereo/suunta (vasen–oikea)?
- [ ] Saako lasien mikrofonin tai kosketuspinnan / Neural Bandin eleet sovelluksen käyttöön?
- [ ] Ray-Ban Display: voiko DAT- tai web-app piirtää näytölle? Millainen web-app-alusta on?

**Säännöt ja tietosuoja**
- [ ] Kamera-LED palaa kuvatessa — onko meidän pakko näyttää muuta suostumusviestiä sivullisille?
- [ ] Saako kuvaa lähettää pilveen (esim. Gemini) vai pitääkö käsittelyn olla paikallista?
- [ ] Ovatko iOS ja Android molemmat tuettuja? Tarvitaanko kehittäjätili tai beta-hyväksyntä etukäteen?

**Arviointi**
- [ ] Testaavatko tuomarit lasit itse? Saako demo olla videona, jos laitteita ei riitä?

## Jos tämä valitaan

- Stack päätetään ja kirjataan (`docs/DECISIONS.md`): mobiilialusta (iOS vai Android, riippuu DAT-vastauksista), tunnistusmalli, pilvikutsut.
- Koodi kansioon `glasses/` (ks. ARCHITECTURE.md).
