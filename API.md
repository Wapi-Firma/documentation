---
title: API
layout: home
nav_order: 3
description: "Usa la API de Wapi"
permalink: /api
---


# Wapi Firma API 📝

Bienvenido a la documentación oficial de la API de **Wapi Firma**. Esta API te permite integrar la firma electrónica de documentos en tus sistemas de forma segura, rápida y legalmente válida, utilizando WhatsApp como canal de comunicación.

**Base URL:** `https://api.wapifirma.com/`

---

## 📄 Solicitar Documento

**Endpoint:** `/api/documentSet`  
**Método:** `POST`  
**Descripción:** Solicita el envío de un documento a firmar.

### Headers

```
x-api-key: your-api-key-here
Content-Type: application/json
```

### Body (ejemplo)

```json
{
  "campaign_id": 1,
  "document": "https://www.w3.org/WAI/ER/tests/xhtml/testfiles/resources/pdf/dummy.pdf",
  "firma_en_todas_las_hojas": true,
  "document_name": "dummy",
  "dni_validation": true,
  "request_photos": true,
  "custom_parameters": {
    "poliza": 210,
    "codigo_interno": "A-21"
  },
  "webhook_url": "https://noApiKey.com",
  "webhook_key": { "key": "value" },
  "wpp": true,
  "datos_firmantes": [
    {
      "name": "Luis Lacoste",
      "dni": "43795269",
      "phone": "5493586005012",
      "position": 1
    },
    {
      "name": "Matias Morales",
      "dni": "12234123",
      "phone": "5491126223122",
      "position": 2
    }
  ]
}
```

### Respuesta exitosa

```json
{
  "id_seguimiento": "d4dc281aceb48e11fa",
  "created_at": 1736996142,
  "urls": [
    {
      "link": "https://firma.wapifirma.com/?rd=F5384BECE0574D809CC129F90C90253B",
      "name": "Luis Lacoste",
      "dni": "43795269",
      "contact": "5493586005012",
      "method": "wpp",
      "id_custom": "F5384BECE0574D809CC129F90C90253B"
    }
  ]
}
```

### Respuestas de error

* **400 Bad Request**

```json
{ "error": ["Missing data or error information"] }
```

* **500 Internal Server Error**

```json
{ "error": "Internal Server Error" }
```

---

## 🗑️ Eliminar Documento

**Endpoint:** `/api/deleteDocument/{id_seguimiento}`
**Método:** `DELETE`
**Descripción:** Elimina un documento no firmado.

### Headers

```
x-api-key: your-api-key-here
Content-Type: application/json
```

### Parámetros

* `id_seguimiento` (string, requerido): ID del documento a eliminar.

### Respuestas

* **200 OK**

```json
{ "success": true, "message": "Document deleted successfully" }
```

* **404 Not Found**

```json
{ "success": false, "message": "No document found with that ID" }
```

* **500 Internal Server Error**

```json
{ "success": false, "message": "Internal server error" }
```

---

## 🗂️ Listar Documentos

**Endpoint:** `/api/list`
**Método:** `GET`
**Descripción:** Devuelve una lista paginada de documentos enviados.

### Headers

```
x-api-key: your-api-key-here
Content-Type: application/json
```

### Parámetros opcionales

* `page`: Número de página.
* `id_seguimiento`: Filtra por ID específico.
* `start_date` / `end_date`: Timestamp UNIX para filtrar por fecha.
* `label`: Filtra por etiqueta.
* `is_finished`: Filtra por estado (true = firmado, false = pendiente).

### Respuesta

```
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

## 📑 Obtener Metadata

**Endpoint:** `/doc/data/{id_seguimiento}`
**Método:** `GET`
**Descripción:** Devuelve toda la metadata del documento, incluyendo el estado de firma, ubicación y huella digital.

### Headers

```
x-api-key: your-api-key-here
Content-Type: application/json
```

### Parámetro

* `id_seguimiento` (string, requerido)

### Respuesta

```
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

## 📄 Descargar Reporte PDF

**Endpoint:** `/api/reporte/{id_seguimiento}`
**Método:** `GET`
**Descripción:** Devuelve el reporte legal del documento firmado en formato PDF binario.

### Parámetro

* `id_seguimiento` (string, requerido)

### Respuestas

* **200 OK:** Devuelve PDF binario.
* **404 Not Found:**

```json
{ "error": "Documento no encontrado" }
```