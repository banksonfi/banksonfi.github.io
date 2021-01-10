---
title: Pankkitilit
---

Kaikki liikenne Banksonin ja pankin välillä on kytketty pankkitiliin.

## Pankkitiliobjekti

```
{
    "id": "<string>",
    "iban": "<IBAN string>",
    "bic": "<BIC string>",
    "contract_id": "<string",
    "created_at": "<timestamp>",
    "updated_at": "<timestamp>",
    "balance": <decimal>,
    "balance_date": "<timestamp>"
}
```

## Pankkitilien listaus

### Pyyntö

`GET /bank-accounts`

Mahdolliset parametrit:

 * `offset` - Monennestako pankkitilistä listaus aloitetaan
 * `limit` - Kuinka monta pankkitiliä listataan

### Vastaus

Sivutettu listaus pankkitiliobjekteja.

## Pankkitilin luonti

### Pyyntö

`POST /bank-accounts`

Pyynnön sisältö: 

```
{
    "certificate": "<pankkivarmenteen id>",
    "iban": "<pankkitilin IBAN>",
    "contract_id": "<mahdollinen maksatustunnus, yleensä Y-tunnus>",
    "customer_information": {
        "name": "<pankkitilin omistaja>",
        "business_id": "<pankkitilin omistajan y-tunnus>",
        "contact_person": "<pankkitilin omistajan yhteyshenkilö>",
        "contact_person_ssn": "<yhteyshenkilön sosiaaliturvatunnus>", 
        "contact_person_email": "<yhteyshenkilön sähköposti>",
        "contact_person_phone": "<yhteyshenkilön puhelinnumero>"
    }
}
```

`customer_information` on pakollinen tieto jos käytetään jaettuja varmenteita. Tätä tietosisältöä käytetään esitäytetyn pankkiyhteysvaltuutuksen luomiseen. Tiedoista ainoastaan yrityksen nimi ja y-tunnus tallennetaan. Jos et halua kenttiä esitäytettyinä, voit lähettää sisältönä välilyönnin (`" "`).

### Vastaus

Luodun pankkitilin objekti tai virhesanoma.

Jos käytettiin jaettuja varmenteita, on vastauksessa myös alkio `authorisation_document` joka sisältää linkin esitäytettyyn pankkiyhteysvaltuutukseen PDF-muodossa.


## Pankkitilin poisto

### Pyyntö

`DELETE /bank-accounts/<id>`

### Vastaus

`204 No content`

## Pankkitilin saldon päivittäminen pankista

### Pyyntö

`POST /bank-accounts/<id>/balance`

### Vastaus

Päivitetyn pankkitilin objekti tai virhevastaus.