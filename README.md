# Logical Entities

## Notification




### Properties Sets

#### Unique identifiers related fields
Notification has two different unique keys a sender related one composed by fields
- senderPaId
- paProtocolNumber
- idempotenceToken

and a native one composed by field
- iun

<details>
  <summary>Schema Fragment</summary>

```yaml
  IUN:
    description: Identificativo Univoco Notifica
    type: string
    minLength: 25
    maxLength: 25
    pattern: ^[A-Z]{4}-[A-Z]{4}-[A-Z]{4}-[0-9]{6}-[A-Z]{1}-[0-9]{1}$
  
  IdsPropertySet:
    type: object
    required:
      - senderPaId
      - paProtocolNumber
      - iun
    properties:
      senderPaId:
        type: string
        description: Identificativo (non IPA) della PA mittente che ha eseguito l'onboarding su SelfCare.
        # wide range of characters
        pattern: ^.*$
        maxLength: 256
      idempotenceToken:
        description: >-
          Identificativo utilizzabile dal chiamante per disambiguare differenti 
          "richieste di notificazione" effettuate con lo stesso numero di protocollo 
          (campo _paProtocolNumber_). Questo può essere necessario in caso di 
          "richiesta di notifica" rifiutata per errori nei codici di verifica degli
          allegati.
        type: string
        # ASCII printable characters
        pattern: ^[ -~]*$
        maxLength: 256
      paProtocolNumber:
        description: >-
          Numero di protocollo che la PA mittente assegna alla notifica stessa
        type: string
        # wide range of characters
        pattern: ^.*$
        maxLength: 256
      iun:
        $ref: '#/components/schemas/IUN'
  ```
</details>


#### Synthetic information
Some fields give an hint on topic related to a notification
- taxonomyCode
- subject
- abstract

<details>
  <summary>Schema Fragment</summary>

  ```yaml
  SyntheticPropertySet:
    type: object
    required:
      - taxonomyCode
      - subject
    properties:
      taxonomyCode:
        type: string
        minLength: 7
        maxLength: 7
        pattern: "^([0-9]{6}[A-Z]{1})$"
        description: >-
          Codice tassonomico della notifica basato sulla definizione presente nell'allegato 2 capitolo C del bando [__AVVISO PUBBLICO MISURA 1.4.5 PIATTAFORMA NOTIFICHE DIGITALI__](https://pnrrcomuni.fondazioneifel.it/bandi_public/Bando/325)
      subject:
        type: string
        description: titolo della notifica
        maxLength: 134
        # wide range of characters
        pattern: ^.*$
      abstract:
        type: string
        description: descrizione sintetica della notifica
        # wide range of characters
        pattern: ^.*$
        maxLength: 1024
  ```
</details>

