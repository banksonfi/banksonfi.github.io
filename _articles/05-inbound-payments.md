---
title: Saapuvat viitemaksut
---

Bankson noutaa ja käsittelee saapuvat viitemaksuaineistot pankista automaattisesti joka tunti.

Uusista maksuista on mahdollista saada notifikaatio käyttäen Webhookseja. On myös mahdollista listata maksuja käyttäen `updated_after` -parametria.

## Saapuvien viitemaksujen objekti

```
{
    "id": "<string>",
    "amount": <number>,
    "debitor_name": "<string>",
    "payment_date": "<date string>",
    "archive_id": "<string>",
    "booking_date": "<date string>",
    "reference_number": "<string>",
    "created_at": "<timestamp>",
    "updated_at": "<timestamp>",
    "bank_account": {<bank account>}
}
```

## Listaus

### Pyyntö

`GET /inbound-payments`

Mahdolliset parametrit:

 * `updated_after=<timestamp>` - listaus palauttaa vain maksut joiden `updated_at` on myöhemmin kuin annettu parametri.
 * `bank_account=<bank account id>` - suodatus pankkitilin id:llä
 * `payment_date_min=<timestamp>` - maksut joiden maksupäivä on annettua parametria myöhemmin
 * `payment_date_max=<timestamp>` - maksut joiden maksupäivä on annettua parametria ennemmin
 * `offset` - sivutettujen tulosten alkupiste
 * `limit` - sivutettujan tulosten määrä per listaus

### Vastaus

Sivutettu listaus maksuobjekteja.