# Idea — Sixth Sense

> Documentation-only concept. Device capabilities, models, user benefits and performance below are hypotheses or targets, not implemented or validated results. Source requirements: [CHALLENGE.md](CHALLENGE.md).

Haaste: **Meta — AI Glasses for Everyone, Everyday**. Audio-first, DAT-mobiili-integraatio.

## One-liner

Sixth Sense kertoo näkövammaiselle kävelijälle äänellä tai värinällä esteestä, joka on tulossa vastaan — erityisesti pään korkeudella, minne keppi ei ylety — ja sanoo rehellisesti, kun se ei ole varma.

## Problem

- Valkoinen keppi tunnustelee maata. **Pään ja rinnan korkeudella** olevat esteet jäävät huomaamatta: avoin kaapinovi, liikennemerkki, oksa, kuorma-auton peili, porrastasanteen alapinta. Näistä tulee pään vammoja.
- Opaskoira auttaa, mutta sitä ei ole useimmilla (pitkä jono, kallis, ei sovi kaikille).
- Puhelinsovellukset (esim. kuvailusovellukset) vaativat, että puhelinta pidetään kädessä suunnattuna eteenpäin. Toinen käsi on jo kepissä.
- Pään asento on luonnostaan "katse eteenpäin". Lasien kamera näkee juuri sen, mihin käyttäjä on menossa, **kädet vapaina**.

## Solution

1. Lasien kamera lähettää kuvavirran puhelimeen (DAT).
2. Puhelin tunnistaa paikallisesti esteet: ihmiset, pylväät, ovet, portaat, pään korkeuden esteet, ajoneuvot. Jokaiselle arvio etäisyydestä ja siitä, onko se **kulkureitillä**.
3. Varoituspolitiikka päättää, sanotaanko jotain: vain lähestyvä este reitillä, tärkein ensin, ei toistoa. Oletuksena **hiljainen**.
4. Ulostulo lasien kaiuttimesta lyhyenä sanana ja suuntana ("mahdollinen este — oikealla"; ei vahvistamatonta askel- tai etäisyysarviota) tai äänimerkkinä, ja/tai värinänä puhelimessa / kellossa.
5. Pyydettäessä (ele tai ääni) pidempi kuvailu näkömallilta: "mitä edessä on?" — selvästi erotettuna turvavaroituksista.
6. Kun järjestelmä ei näe (pimeä, sumea kuva, yhteys katki), se **sanoo sen ääneen**: "en näe nyt". Hiljaisuus ei koskaan tarkoita "reitti on vapaa".

## Target user

| | |
|---|---|
| Primary | Sokea tai heikkonäköinen aikuinen, joka liikkuu itsenäisesti kepin kanssa tutussa ympäristössä (koti, työpaikka, lähikauppa, asema) |
| Secondary | Heikkonäköinen ilman keppiä; ikääntynyt, jonka näkö heikkenee |
| Not for | Kepin tai opaskoiran korvaaminen; liikenteen ylitys; navigointi paikasta A paikkaan B |

## Why now

- DAT avaa Metan lasien kameran ja äänen ulkopuolisille sovelluksille. Ennen tätä tämä vaati erikoislaitteen.
- Pienet tunnistusmallit pyörivät puhelimessa reaaliajassa; pilveä ei tarvita turvavaroituksiin.

## Differentiation

| Vaihtoehto | Miksi me |
|---|---|
| Valkoinen keppi | Täydennämme, emme korvaa: katamme pään korkeuden |
| Puhelimen kuvailusovellukset | Kädet vapaina, jatkuva eikä pyydettävä, sekunnin murto-osassa |
| Etäavustajapalvelut (vapaaehtoinen videopuhelu) | Ei tarvita toista ihmistä, toimii ilman verkkoa |
| Ultraääni-"älykepit" | Tietää *mikä* este on, ei vain *että* jotain on |
| Lasien oma "Hey Meta, mitä edessäni on?" | Varoittaa itse, pyytämättä; ei sekunttien viivettä |

## Demo scenario

Katso [docs/DEMO.md](docs/DEMO.md). Demo tehdään hallitusti, näkevänä ja valvottuna pehmeillä rekvisiitoilla; ei unimaskirataa, portaita tai liikennettä. Näytä ensimmäisen persoonan näkymä, lyhyt vihje, epävarmuus sekä lupa- ja yhteysvirheistä palautuminen. Älä anna tuomarin tai käyttäjän tukeutua prototyyppiin liikkumisturvallisuudessa.

Laitetila merkitään näkyviin: oikeat lasit, puhelinkamera tai tallennetoisto. Varapolku ei todista toimivaa lasi-integraatiota. Luvut näytetään vain mitattuina ja olosuhteet kerrotaan.

## Scope

### Must have (demo breaks without it)
- [ ] Kuvavirta laseista puhelimeen DAT:lla (tai varapolku: puhelimen kamera rintataskussa, sanotaan ääneen)
- [ ] Paikallinen tunnistus: ihminen, pylväs/tolppa, ovi, portaat, pään korkeuden este
- [ ] "Onko reitillä ja kuinka lähellä" -arvio + varoituspolitiikka (ei hälytystulvaa)
- [ ] Ääniulostulo laseista; "en näe nyt" -tila
- [ ] Laitteen tilat: lasit pois päästä, yhteys katki, akku vähissä, kameralupa evätty
- [ ] Mitattu arviointi omalla merkityllä videoaineistolla (ks. Metrics)

### Should have (judges notice)
- [ ] Suunta äänessä (stereo vasen/oikea) tai värinäkuvio puhelimessa/kellossa
- [ ] "Mitä edessä on?" -kuvailu näkömallilla pyydettäessä
- [ ] Kasvojen sumennus ennen kuin mikään kuva lähtee puhelimesta
- [ ] Testi 2–3 näkövammaisen tai näkövammaisjärjestön edustajan kanssa (palaute deckiin)
- [ ] Ray-Ban Display: suurikontrastinen suuntanuoli heikkonäköisille

### Won't have (sanotaan ääneen, se lisää uskottavuutta)
- [ ] Navigointi (reitit, kartat)
- [ ] Liikennevalot ja kadun ylitys — liian suuri riski väärälle varmuudelle
- [ ] Kepin tai opaskoiran korvaaminen
- [ ] Videon tallennus tai pilvitallennus
- [ ] Kasvojen tunnistus (kuka ihminen on)

## Risks

| Riski | Todennäköisyys | Lievennys |
|---|---|---|
| **Väärä varmuus**: käyttäjä luottaa hiljaisuuteen ja törmää | Suuri | Hiljaisuus ≠ vapaa reitti, sanotaan onboardingissa; "en näe" -tila; aina kepin rinnalla; ei koskaan "reitti vapaa" -viestiä |
| DAT ei anna jatkuvaa videota tai latenssi > 1 s | Keskisuuri | Kysytään Metalta ensimmäisenä (README); varapolku puhelimen kameralla; jos > 1 s, rajataan hitaasti liikkuviin tilanteisiin ja kerrotaan se |
| Väärät hälytykset → käyttäjä lakkaa kuuntelemasta | Suuri | Varoitus vain reitillä + lähestyvä; vaimennus samalle esteelle; mitataan väärät hälytykset / min ja viritetään kynnys sen mukaan |
| Portaat alas ja syvyys yhdestä kamerasta | Suuri | Monokulaarinen syvyysarvio vain karkeasti (lähellä / keskellä / kaukana); portaat alas "varoitus" eikä etäisyys; kirjataan heikkoudeksi |
| Laitteita liian vähän / jaettu muiden kanssa | Keskisuuri | Nauhoitetaan videoaineisto heti kun lasit saadaan; kehitys nauhoitusta vasten |
| Sivullisten yksityisyys | Keskisuuri | Ei tallennusta, käsittely muistissa, kasvojen sumennus ennen pilveä, LED näkyy |
| Akku ja lämpeneminen | Keskisuuri | Mitataan; kuvanopeuden säätö kävelyvauhdin mukaan |

## Metrics (what "works" means)

Omalla nauhoitetulla ja käsin merkityllä aineistolla (eri tilat, valaistus, ihmiset), jako ajon mukaan: opetus/viritys yhdellä joukolla, luvut toisella.

- **Hälytysten recall reitillä olevista esteistä**, kun ne ovat 1–3 m päässä — pääluku. Erikseen pään korkeuden esteille.
- **Väärät hälytykset per kävelyminuutti** (tavoite < 1).
- **Latenssi** kamerasta ääneen, p50 ja p95 (tavoite p95 < 500 ms).
- **"En näe" -tilan osuus** ajasta — rehellisyys ennen näennäistä tarkkuutta.
- Käyttäjäpalaute: ohjauksen saavutettavuus, vihjeiden ymmärrettävyys ja luottamus hallitussa tilanteessa suostumuksella. Ei törmäyskokeita tai unimaskilla mitattua turvallisuusväitettä.
