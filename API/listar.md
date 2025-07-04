---
title: Listar Documentos
layout: home
parent: API
nav_order: 3
description: "Devuelve una lista paginada de documentos enviados."
---
Devuelve una lista paginada de documentos enviados.
# 🗂️ Listar Documentos

**Endpoint:** `/api/list`  
**Método:** `GET`

## Headers

```
x-api-key: your-api-key-here
Content-Type: application/json
```

## Parámetros

* `page`: Número de página.
* `id_seguimiento`: Filtra por ID específico.
* `start_date` / `end_date`: Timestamp UNIX para filtrar por fecha.
* `label`: Filtra por etiqueta.
* `is_finished`: Filtra por estado (true = firmado, false = pendiente).

## Respuesta

```json
[
  {
    "id_seguimiento": "a55d68a337d91db31d",
    "uploaded_pdf": "https://wapi-doc.s3.amazonaws.com/a55d68a337d91db31d.pdf",
    "is_finished": false,
    "finished_at": null,
    "cantidad_firmantes": 2,
    "cantidad_firmados": 0,
    "created_at": 1736997551,
    "client": "custom-api",
    "firmantes": [
      {
        "name": "Luis Lacoste",
        "dni": "43795269",
        "signed": false,
        "phone": "5493586005012",
        "method": "wpp",
        "id_custom": "22CC93E5965F48D5BD22C06BE4B7A36D",
        "link": "https://firma.wapifirma.com/?rd=22CC93E5965F48D5BD22C06BE4B7A36D",
        "delivered": true
      },
      {
        "name": "Matias Morales",
        "dni": "41595469",
        "signed": false,
        "phone": "5493586005013",
        "method": "wpp",
        "id_custom": "22CC93E5965F48D5BD22C06BE4B7A36D_2",
        "link": "https://firma.wapifirma.com/?rd=22CC93E5965F48D5BD22C06BE4B7A36D_2",
        "delivered": false
      }
    ],
    "webhook": "https://noApiKey.com",
    "id_custom_client": "dummy",
    "label": {
      "label_name": null,
      "color": null
    },
    "custom_parameters": {
      "nombre": "test",
      "date": 1,
      "list": [
        "nombre",
        1
      ]
    }
  }
]
```

---
