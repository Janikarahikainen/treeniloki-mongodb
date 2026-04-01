# Tietokantakyselyt

Ennen kuin ajat alla olevia komentoja MongoDB Compassissa tai Shellissä, varmista että olet valinnut oikean tietokannan komennolla:

```
use treeniloki
```

## Datan haku (Read)

### Hakee kaikki Matin treenit ja järjestää ne uusimmasta vanhimpaan

```
db.treenit.find({
kayttaja_id: ObjectId("605c72ef2f753b21c4000001")
}).sort({ paivamaara: -1 });
```

### Ryhmittelee treenit tyypin mukaan ja laskee niiden määrän

```
db.treenit.aggregate([
{
$group: {
_id: "$treenityyppi",
maara: { $sum: 1 }
}
},
{ $sort: { maara: -1 } }
]);
```

### Yhdistää treenidatan ja käyttäjätiedot yhteen näkymään

```
db.treenit.aggregate([
{
$lookup: {
from: "kayttajat",
localField: "kayttaja_id",
foreignField: "_id",
as: "kayttaja"
}
},
{ $unwind: "$kayttaja" }, // Purkaa taulukon objektiksi
{
$project: {
"kayttaja.nimi": 1,
treenityyppi: 1,
paivamaara: 1,
kesto_min: 1, _id:0
}
}
]);
```

## Datan lisäys (Create)

### Lisää Matille treenin

```
db.treenit.insertOne({
"kayttaja_id": ObjectId("605c72ef2f753b21c4000001"), // Matin ID
"paivamaara": new Date(), // Tallentaa tämän hetken
"treenityyppi": "Yläkroppa",
"kesto_min": 45,
"liikkeet": [
{
"nimi": "Hauiskääntö",
"sarjat": [
{ "toistot": 12, "paino_kg": 15 },
{ "toistot": 10, "paino_kg": 15 }
]
}
],
"muistiinpanot": "Lyhyt ja ytimekäs treeni."
});
```

## Datan muokkaus (Update)

### Lisää uusi sarja olemassa olevaan liikkeeseen

```
db.treenit.updateOne(
{
"kayttaja_id": ObjectId("605c72ef2f753b21c4000001"),
"liikkeet.nimi": "Kyykky"
},
{
$push: { "liikkeet.$.sarjat": { "toistot": 5, "paino_kg": 100 } }
}
);
```

## Datan poisto (Delete)

### Poistaa yksittäisen treenin ID:n perusteella

```

db.treenit.deleteOne({
"\_id": ObjectId("69cd41c80b1e13c770724b6f")
});
```
