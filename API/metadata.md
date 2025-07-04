---
title: Metadata
layout: home
parent: API
nav_order: 4
description: "Devuelve toda la metadata del documento, incluyendo el estado de firma, ubicación y huella digital."
---
Devuelve toda la metadata del documento, incluyendo el estado de firma, ubicación y huella digital.
# 📑 Obtener Metadata

**Endpoint:** `/doc/data/{id_seguimiento}`  
**Método:** `GET`

## Parámetros

- **`id_seguimiento` (string, requerido)**

## Headers

```
x-api-key: your-api-key-here
Content-Type: application/json
```

## Respuesta

```json
{
  "file_url": "https://wapi-doc.s3.amazonaws.com/d4dc281aceb48e11fa.pdf",
  "is_finished": true,
  "finished_at": "2025-01-15 16:26",
  "signers": [
    {
      "name": "Luis Lacoste",
      "dni": "41595469",
      "wpp": "5493586005012",
      "foto_dni": "https://wapi-img.s3.us-east-2.amazonaws.com/23328382D269406FA78AFFF5172F2670ci.jpeg",
      "foto_frontal": "https://wapi-img.s3.us-east-2.amazonaws.com/23328382D269406FA78AFFF5172F2670sf.jpeg",
      "signature": {
        "hardwareData": [
          "iPhone",
          "Gecko",
          "Mozilla/5.0 (iPhone; CPU iPhone OS 18_2 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/18.2 Mobile/15E148 Safari/604.1"
        ],
        "hash": "SHA2-512 PDF: 0D01F599D4B23FD6E65D7BC1111E6AD71D37C117DF35C289F7E7C02707651BEB8B58FB2F21F58283A8C5802BB82976B143B991FD914041E95AD184EDA5CA1629",
        "coords": "-32.12345632346234,-51.23151231237813235",
        "time": "2025-01-15T13:26:55",
        "img": "B64 image of the signature",
        "IpAddress": "192.168.1.1"
      }
    }
  ],
  "created_at": "2025-01-15 19:21",
  "doc_name": "0dc55b8636eec8b0e8",
  "dni_validation": false,
  "method": false,
  "edatalia": "91628382D269406FA78AFFF5172F2670",
  "hash": "0D01F599D4B23FD6E65D7BC1111E6AD71D37C117DF35C289F7E7C02707651BEB8B58FB2F21F58283A8C5802BB82976B143B991FD914041E95AD184EDA5CA1629",
  "auditTrail": [
    {
      "eventDateTime": "2025-01-15T16:26:45.9479077Z",
      "eventType": "RecipientInProcess",
      "recipientId": "Luis Lacoste",
      "documentId": null,
      "data": {}
    },
    {
      "eventDateTime": "2025-01-15T16:26:49.4709513Z",
      "eventType": "DocumentOpened",
      "recipientId": "Luis Lacoste",
      "documentId": "f8b7b50cf5dfb27064",
      "data": {
        "IpAddress": "192.168.1.1",
        "UserAgent": "Mozilla/5.0 (iPhone; CPU iPhone OS 18_2 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/18.2 Mobile/15E148 Safari/604.1"
      }
    },
    {
      "eventDateTime": "2025-01-15T16:26:49.471595Z",
      "eventType": "PageRead",
      "recipientId": "Luis Lacoste",
      "documentId": "f8b7b50cf5dfb27064",
      "data": {
        "IpAddress": "192.168.1.1",
        "UserAgent": "Mozilla/5.0 (iPhone; CPU iPhone OS 18_2 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/18.2 Mobile/15E148 Safari/604.1",
        "PageNumber": "1"
      }
    },
    {
      "eventDateTime": "2025-01-15T16:26:50.2900035Z",
      "eventType": "DocumentRead",
      "recipientId": "Luis Lacoste",
      "documentId": "f8b7b50cf5dfb27064",
      "data": {
        "IpAddress": "192.168.1.1",
        "UserAgent": "Mozilla/5.0 (iPhone; CPU iPhone OS 18_2 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/18.2 Mobile/15E148 Safari/604.1"
      }
    },
    {
      "eventDateTime": "2025-01-15T16:26:52.367272Z",
      "eventType": "DocumentAccepted",
      "recipientId": "Luis Lacoste",
      "documentId": "f8b7b50cf5dfb27064",
      "data": {
        "IpAddress": "192.168.1.1",
        "UserAgent": "Mozilla/5.0 (iPhone; CPU iPhone OS 18_2 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/18.2 Mobile/15E148 Safari/604.1",
        "Gps": "Latitud: -32.12345632346234°, Longitud: -51.23151231237813235° (Precisión: 20.218732691913118m)"
      }
    },
    {
      "eventDateTime": "2025-01-15T16:26:56.9106452Z",
      "eventType": "DocumentSigned",
      "recipientId": "Luis Lacoste",
      "documentId": "f8b7b50cf5dfb27064",
      "data": {}
    },
    {
      "eventDateTime": "2025-01-15T16:26:58.9088614Z",
      "eventType": "DocumentTimestamped",
      "recipientId": "Luis Lacoste",
      "documentId": "f8b7b50cf5dfb27064",
      "data": {}
    }
  ]
}
```

---
