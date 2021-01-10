---
title: "Johdanto"
---

Banksonin rajapinta löytyy osoitteesta `https://mb.bankson.fi/api/v2`

Rajapinta vastaanottaa ja lähettää dataa pääsääntöisesti JSON-muodossa.

Versiossa kaksi APIin on lisätty mahdollisuus käyttää Bankson Oy:n pankkiyhteyskanavia suomalaisten pankkien pankkiliikenteeseen. Tämä on myös suositeltu tapa. Bankson tukee edelleen teknisesti omien varmenteiden noutoa ohjelmaan.

## Testaus

APIn versiossa kaksi testaus tapahtuu tuotantovarmenteilla testimoodissa. Testimoodi on saatavilla vain lähetettävissä aineistoissa. Testimoodi lähetyksessä laitetaan päälle määrittämällä HTTP Header:

`"X-Bankson-Environment: test"`

## Autentikaatio

Rajapinnan autentikaatio perustuu RSA-avaimiin.

Käyttääksesi rajapintaa tulee sinun ensin luoda API-avain käyttöliittymästä:

![api-keys-screenshot](/assets/img/apikeys.png)

Muista tallentaa avaimen id, julkinen avain sekä yksityinen avain.

Käyttäen avaimia voidaan muodostaa autentikaatio HTTP header. Se on muotoa:

`Authorization: BanksonRSA ApiKey=<apikeyid>, Timestamp=<current_timestamp>, Signature=<calculated signature>`

Allekirjoitus lasketaan seuraavalla kaavalla:

`Signature = privateKey.sign(“<apikeyid><current_timestamp>”)`

Signature tulee laittaa HTTP headeriin base64 muodossa.

Esimerkki allekirjoituksen laskennasta JavaScript-muodossa löytyy osoiteesta https://github.com/banksonfi/bankson-js/blob/master/src/client.js#L49-L56

## Vastaustyypit

Bankson API vastaa pääsääntöisesti JSON-muodossa olevilla vastuksilla.

Yksittäisten resurssien vastaukset ovat aina JSON-objekteja joilla on aina `id` –avain joka on muotoa `string`.

Listausten vastaukset ovat aina JSON-objekteja. Osa listauksista on sivutettu. Sivuttamattomat listaukset ovat muotoa:

```
{
    "paginated": false,
    "items": [
        {"id": "<string>"},
        {"id": "<string>"}
    ]
}
```

Sivutetut vastaukset ovat muotoa:

```
{
    "total_count": <integer>,
    "items": [
        {"id": "<string>"},
        {"id": "<string>"}
    ]
}
```
