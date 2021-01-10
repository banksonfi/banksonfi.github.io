---
title: Lähtevät maksut
---

Banksonilla on mahdollista luoda viitemaksuja. Laskut odottavat Banksonissa maksupäivään asti, jonka jälkeen ne siirretään pankin järjestelmään. Tällä tavalla on mahdollista perua maksut maksupäivään asti ja säästetään pankkien palvelumaksuissa, kun maksut lähtevät yhdessä aineistossa. Mikäli maksu luodaan Banksoniin samana päivänä kuin sen maksupäivä, siirretään se pankkiin n. 15 minuutin viiveellä. Tällä tavalla on mahdollista niputtaa myös samana päivänä maksettavia laskuja yhteen aineistoon.

Bankson myös noutaa pankista 15 minuutin välein maksuaineistojen palautteita. Riippuen pankista ja pankkisopimuksista pankista voidaan saada palaute siitä, mikä maksun tila on pankin järjestelmässä. Kaikkien pankkien osalta hylkääntyneet maksut muodostavat palautteen joka noudetaan Banksoniin ja sitä käyttäen maksun tila merkataan epäonnistuneeksi.

Maksun tilan muutoksia on mahdollista seurata Webhookseilla tai listaamalla maksut käyttäen `updated_after` parametria.

## Maksuobjekti

```
{
    "id": "<string>",
    "amount": <number>,
    "recipient_name": "<string>",
    "recipient_iban": "<IBAN string>",
    "recipient_bic": "<BIC string>",
    "payment_date":"<date str>",
    "reference_number": "<string>",
    "vendor_reference": "<string>",
    "end_to_end_id": "<string>",
    "payment_information_id": "<string>",
    "message_id": "<string>",
    "created_at": "<timestamp>",
    "updated_at": "<timestamp>",
    "status": "<string>",
    "status_details": "<string>",
    "bank_account": {<bank_account>}
}
```

Maksujen tilat (`status`):

 * `ACTC` - Accepted technical validation - maksu on muodollisesti oikein
 * `ACCP` - Accepted content processing - asiakkaan sopimus on tarkistettu
 * `ACSC` - Accepted settlement completed - maksu on hyväksytty maksatukseen
 * `ACSP` - Processed - maksu on käsitelty
 * `PNDG` - Pending for processing - maksu odottaa käsittelyä
 * `RJCT` - Rejected - maksu on hylätty. Pankin antama lisätieto löytyy `status_details` kentästä
 * `__WAIT` - Banksonin sisäinen tila - maksu odottaa siirtoa pankin järjestelmään

## Maksujen listaus

### Pyyntö

`GET /outbound-payments`

Mahdolliset parametrit:

 * `updated_after=<timestamp>` - listaus palauttaa vain maksut joiden `updated_at` on myöhemmin kuin annettu parametri.
 * `bank_account=<bank account id>` - suodatus pankkitilin id:llä
 * `payment_date_min=<timestamp>` - maksut joiden maksupäivä on annettua parametria myöhemmin
 * `payment_date_max=<timestamp>` - maksut joiden maksupäivä on annettua parametria ennemmin
 * `offset` - sivutettujen tulosten alkupiste
 * `limit` - sivutettujan tulosten määrä per listaus

### Vastaus

Sivutettu listaus maksuobjekteja.

## Maksujen luonti

### Pyyntö

`POST /outbound-payments`

Pyynnön sisältö on joko lista maksuja tai yksittäinen maksuobjekti.

Maksuobjektin pakolliset parametrit ovat

 * `source` - pankkitilin id, josta maksu maksetaan
 * `recipient_name` - maksunsaajan nimi
 * `recipient_iban` - maksunsaajan IBAN-tilinumero
 * `recipient_bic` - maksunsaajan BIC
 * `amount` - maksun summa
 * `reference_number`- viitenumero
 * `payment_date` - maksupäivä muodossa `YYYY-MM-DD`

Vapaaehtoiset paremetrit:

 * `vendor_reference` - vapaamuotoinen merkkijono jolla voidaan yksilöidä maksua integroitavan sovelluksen tiedoilla. Esimerkiksi maksun / ostolaskun id-numero.

### Vastaus

 ```
 {
     "succeeded": [<lista onnistuneita maksuobjekteja>],
     "failed": [<lista epäonnistuneita maksuobjekteja>],
     "success_count": <number>,
     "failed_count": <number>
 }
 ```

## Odottavan maksun poisto

Kun maksun tila on `__WAIT`, se voidaan poistaa banksonista jolloin sitä ei siirretä pankkiin maksupäivänä.

### Pyyntö

`DELETE /outbound-payments/<id>`

### Vastaus

`204 No content` tai virhevastaus.