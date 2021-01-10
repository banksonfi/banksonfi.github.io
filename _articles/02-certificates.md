---
title: Pankkivarmenteet
---

Bankson tunnistautuu pankkien järjestelmiin käyttäen yrityskohtaisia pankkivarmenteita. Jos halutaan käyttää jaettuja varmenteita, bankson tunnistautuu aina Bankson Oy:n varmenteilla, jolloin täytyy pankissa valtuuttaa Bankson Oy välittämään pankkiaineistoja.

## Varmenneobjekti

Varmenneobjekteja on kahta tyyppiä: jaettu varmenne ja oma varmenne

Käytettäessä Bankson Oy:n varmenteita pankkiyhteysvaltuutuksella varmenneobjekti on muotoa:

```
{
    "id": "<string>",
    "shared": true,
    "bic": "<BIC string>"
}
```

Käytettäessä omia varmenteita varmenneobjekti on muotoa:


```
{
    "id": "<string>",
    "bank_customer_id": "<string>",
    "bank_target_id": "<string>",
    "bic": "<BIC string>",
    "created_at": "<timestamp>",
    "updated_at": "<timestamp>",
    "not_after": "<timestamp>",
    "not_before": "<timestamp>",
    "subject": "<string>"
}
```

## Varmenteiden listaus

### Pyyntö

`GET /bank-certificates`

### Vastaus

Listaus varmenneobjekteja