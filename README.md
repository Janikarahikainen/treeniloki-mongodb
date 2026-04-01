# Treeniloki (MongoDB)

Tämä projekti on harjoitustyö, jossa on suunniteltu ja toteutettu tietokantaratkaisu kuntosaliharjoittelun seurantaan käyttäen MongoDB-NoSQL-tietokantaa.

## Casen kuvaus

Treeniloki on sovellus, johon käyttäjä voi kirjata ylös kuntosalitreenejään. Tässä mallissa hyödynnetään MongoDB:n joustavuutta:

- Referencing: Käyttäjät ovat omassa kokoelmassaan, ja treenit viittaavat käyttäjän ID:hen.

- Embedding: Yksittäinen treeni kaikkine liikkeineen ja sarjoineen tallennetaan yhtenä dokumenttina.

Tämä rakenne tekee datan lukemisesta nopeaa ja mahdollistaa erilaisten treenityyppien (esim. voima vs. jooga) tallentamisen ilman jäykkää taulukkorakennetta.

## Tietomalli

Tietokanta koostuu kahdesta kokoelmasta:

- **kayttajat**: Sisältää käyttäjän perustiedot (nimi, sähköposti).

- **treenit**: Sisältää treenidatan.

## Ohjeet: Ympäristön pystytys ja testaus

Seuraa näitä vaiheita ladataksesi ja ajaaksesi projektin omalla koneellasi.

### 1. Lataa projekti GitHubista

- Avaa terminaali ja kopioi projektin tiedostot itsellesi:

```
git clone https://github.com/Janikarahikainen/treeniloki-mongodb.git
```
```
cd treeniloki-mongodb
```

### 2. Tietokannan käynnistys

- Varmista, että Docker Desktop on käynnissä.
- Suorita kansiossa komento:

```
docker-compose up -d
```

- Tämä käynnistää MongoDB-kontin taustalle porttiin **27017**.

### 3. Datan tuonti (MongoDB Compass)

- Avaa **MongoDB Compass** ja yhdistä osoitteeseen: **mongodb://localhost:27017**
- Luo uusi tietokanta nimeltään **treeniloki**.
- Luo kaksi kokoelmaa (Collection): **kayttajat** ja **treenit**.
- Tuo JSON-tiedostot kokoelmiin:
  - Valitse kayttajat-kokoelma -> Add Data -> Import JSON -> valitse kayttajat.json.
  - Valitse treenit-kokoelma -> Add Data -> Import JSON -> valitse treenit.json.

### 4. Kyselyiden testaaminen

Kyselyt ja niiden selitykset löytyvät tiedostosta **queries.md**. Voit kopioida sieltä komentoja ja ajaa niitä Compassin Mongosh-näkymässä. Vaihda oikeaan tietokantaan komennolla:

```
use treeniloki
```
