# Architecture

> Alustava suunnitelma. DAT:n todelliset rajat (videovirta, latenssi, ääni) varmistetaan Metan pisteellä, ks. `README.md`.

## Runtime shape

Tavoite: tilannevihjeiden käsittely **puhelimessa ilman verkkoyhteyttä**. Tämä ei ole toteutettu tai mitattu ominaisuus. Pilveä harkitaan vain erikseen pyydettyyn ja suostuttuun kuvailuun.

```mermaid
flowchart LR
    G[Lasit<br/>kamera, kaiutin, mikki,<br/>kosketus / Neural Band] -->|DAT: kuvavirta + tilat| R[A: Laiteyhteys<br/>luvat, yhteys, tilat,<br/>kuvanopeuden säätö]
    R -->|Frame M1| P[B: Havainnointi<br/>tunnistus, syvyys,<br/>seuranta, reitti]
    R -->|DeviceState M2| W
    P -->|Hazard-lista M3| W[C: Varoituspolitiikka<br/>mitä, milloin, miten]
    W -->|Alert M4| O[A: Ulostulo<br/>ääni laseihin, värinä]
    O --> G
    G -->|ele / ääni| R -->|Request M5| D[C: Kuvailu<br/>kasvojen sumennus → näkömalli]
    D -->|teksti| O
```

## Components

| Komponentti | Omistaja | Vastuu |
|---|---|---|
| Laiteyhteys | A | DAT-istunto, luvat, kuvavirta, lasien tilat (päässä / pois, kotelo, akku, BT-katko), kuvanopeuden säätö |
| Ulostulo | A | Puhe ja äänimerkit lasien kaiuttimeen, värinä puhelimessa/kellossa, jono ja keskeytys |
| Nauhoitus + toisto | A | Kehitystila: virta tallennetaan paikallisesti ja toistetaan putkeen kuin elävä virta |
| Havainnointi | B | Objektintunnistus, pään korkeuden esteet, karkea syvyys, seuranta kuvasta toiseen, "onko reitillä" |
| Arviointi | B | Merkitty aineisto, jako, `metrics.json`, latenssimittaus |
| Varoituspolitiikka | C | Priorisointi, vaimennus, epävarmuuden viestintä, "en näe" -tila, sanasto |
| Kuvailu + tietosuoja | C | Kasvojen sumennus, näkömallikutsu, välimuisti, offline-varapolku |
| Käyttäjätesti | C | Rata, protokolla, suostumuslomake, tulokset |

## Latenssibudjetti (tavoite p95 < 500 ms)

| Vaihe | Budjetti | Huom. |
|---|---|---|
| Lasien kamera → puhelin (DAT) | ~150 ms | **Tuntematon — varmista Metalta** |
| Tunnistus puhelimessa | ≤ 80 ms | Pieni malli (YOLO-luokka, CoreML / TFLite), 320–416 px |
| Syvyys + seuranta + reitti | ≤ 40 ms | Karkea; voi ajaa harvemmin kuin tunnistus |
| Varoituspolitiikka | ≤ 5 ms | Puhdas funktio |
| Puhe laseihin | ~150 ms | Valmiiksi syntetisoidut lyhyet sanat, ei TTS:ää lennosta |

Latenssibudjetti on suunnittelutavoite, ei turvallisuuden raja tai mitattu tulos. Monokulaarinen kuva ei yksin takaa luotettavaa metristä etäisyyttä. Älä lupaa tiettyä varoitusetäisyyttä tai vapaata reittiä.

## Havainnointi (B)

1. **Tunnistus:** valmiiksi opetettu pieni malli (COCO: ihminen, polkupyörä, auto, tuoli, penkki…) + hienosäätö omilla luokilla: pylväs/tolppa, ovi, portaat ylös/alas, pään korkeuden este. Oman merkityn aineiston riittävyys on selvitettävä; hienosäädön hyötyä ei oleteta.
2. **Pään korkeus:** este, jonka alareuna on kuvassa horisontin yläpuolella ja joka kasvaa kohti keskikohtaa. Ei tarvitse metrisiä etäisyyksiä.
3. **Syvyys:** monokulaarinen syvyysmalli (pieni) tai laatikon koko + kasvunopeus → *lähellä / keskellä / kaukana* ja *lähestyy / ei*. Ei metrejä, jos ne eivät ole luotettavia.
4. **Reitti:** kuvan keskikaista ≈ kävelysuunta. Lasit liikkuvat pään mukana → tasoitus usean kuvan yli; kiihtyvyysanturi, jos DAT antaa sen.
5. **Seuranta:** sama este saa saman `track_id`:n kuvasta toiseen, jotta C voi vaimentaa toistot.

## Varoituspolitiikka (C)

- Varoitetaan vain, jos `in_path` ja (`proximity=near` tai `approaching`). Muuten hiljaa.
- Yksi varoitus kerrallaan, tärkein ensin: pään korkeus > portaat alas > ajoneuvo > pylväs > ihminen > ovi.
- Sama `track_id` vaimennetaan N sekunniksi, ellei se tule selvästi lähemmäs.
- Epävarmuus kerrotaan sanalla tai eri äänellä ("ehkä portaat"), ei piiloteta.
- `DeviceState` voittaa kaiken: kamera peitossa, pimeä, yhteys poikki → "en näe nyt" + värinä, toistetaan harvakseltaan.
- Kuvailu (näkömalli) ei koskaan keskeytä turvavaroitusta, ja turvavaroitus keskeyttää kuvailun.

## Tietosuoja ja suostumus

- Kuvat käsitellään muistissa, ei tallenneta. Nauhoitus vain kehitystilassa, tiimin omalla radalla, suostumuksella.
- Ennen kuin kuva lähtee puhelimesta (kuvailu), kasvot sumennetaan paikallisesti.
- Onboarding (äänellä): mitä sovellus tekee, mitä se ei tee, hiljaisuus ≠ vapaa reitti.

## Esitetty kansiorakenne (jos valitaan)

```
glasses/
  app/          A  mobiilisovellus (iOS tai Android, DAT)
  perception/   B  mallit, vienti CoreML/TFLite, arviointi
  policy/       C  varoituspolitiikka puhtaana logiikkana + testit
  describe/     C  kuvailu + kasvojen sumennus
  data/         B  merkityt klipit (ei kasvoja repoon)
```


## Challenge alignment and implementation gates

The supplied brief requires a functional prototype and deck, inclusive interaction, consent, safe data handling, permissions, errors, connectivity, performance trade-offs and device-state changes. See [CHALLENGE.md](CHALLENGE.md) and [docs/JUDGING.md](docs/JUDGING.md).

No runtime or SDK has been selected. Validate video/still access, capture timestamps, audio routing, state events and offline operation with Meta before selecting iOS, Android or web. Do not assume a wearable's camera direction always equals walking direction.

M1–M7 remain proposed contracts. Add explicit monotonic timestamps and a stale-frame/watchdog policy during contract review. Permission denied/revoked, disconnected, stale/obstructed input, low battery and unsupported hardware must produce distinct, accessible states where observable. State uncertainty remains visible; unsupported SDK events must be documented rather than simulated as device evidence.

Configurable speech, repeat/mute, optional non-audio feedback and an accessible recovery path should be validated with intended users. Keep environmental audio available; do not promise hardware haptics or stereo until confirmed. Description output cannot be allowed to imply a guaranteed safe route.

Cloud description is optional. Consent and local redaction must happen before any upload; face blur alone does not anonymise a scene. Fail closed for upload when consent or redaction is unavailable, and keep a local explanation/failure path. Consent-based development recordings belong in restricted storage with a retention/deletion plan, not public Git.

Use controlled, sighted/supervised demonstrations and stationary soft-prop tests as described in [docs/DEMO.md](docs/DEMO.md). No blindfold navigation, traffic or stair trials relying on the prototype. Record latency, missed/false events, unavailable time and device mode without turning these into a mobility-safety guarantee.
