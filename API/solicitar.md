---
title: Solicitar Documento
layout: home
parent: API
nav_order: 1
description: "Solicita el envío de un documento a firmar."
---
# 📄 Solicitar Documento
Solicita el envío de un documento a firmar.

**Endpoint:** `/api/documentSet`  
**Método:** `POST`  

## Attributes
- ### Document Settings - Object
  - **campaign_id (number, required)**  - The ID of the campaign to associate the document with.
  - **document (string, required)**  - The public URL or B64 of the document to be signed.
  - **firma_en_todas_las_hojas (boolean, optional)**  - If `true`, the signature will be applied to all pages of the document. Default is `False`.
  - **document_name (string, optional)**  - The name of the document. Default is `Wapi firma document`.
  - **dni_validation (boolean, optional)**  - Indicates whether to validate the DNI of signers. Default is `False`.
  - **request_photos (boolean, optional)**  - Indicate whether to ask for photo selfie of signrer. Default is `False`.
  - **custom_parameters (object, optional)**  -  A flexible field for custom metadata related to the document. Default is `null`.
- ### Communication - Object
  - **webhook_url (string, optional):** The webhook URL to send a callback when the signing process is complete.
    - Body: The structure of the callback payload.
      - Example
        ```json
        {
          "type": "alert",
          "data": {
            "id_seguimiento": "d4dc281aceb48e11fa",
            "documento_url": "https://wapi-doc.s3.amazonaws.com/d4dc281aceb48e11fa.pdf",
            "is_finished": true,
            "finished_at": "2025-01-23 03:12:38.478962+00",
            "custom_parameters": {
              "poliza": 210,
              "codigo_interno": "A-21"
            },
            "report_url": "https://api.wapifirma.com/api/reporte/d4dc281aceb48e11fa"
          }
        }
        ```
  - **webhook_key (object, optional):** The key-value pair for the webhook URL, if needed.
      - Example
        ```json
        {
          "key": "value"
        }
        ```
  - **wpp (boolean, optional):** Indicates if a WhatsApp notification should be sent to the signer. Default is `True`.

### datos_firmantes (array[], required) - The array of signers. Each object contains:
+ name (string, required) - The name of the signer.
+ dni (string, required) - The DNI of the signer.
+ phone (string, required) - The phone number of the signer.
+ position (string, optional) -  The signing order or position within the document (used to determine placement).

## Headers

```
x-api-key: your-api-key-here
Content-Type: application/json
```
## Body (ejemplo)

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

## Respuesta exitosa

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

## Respuestas de error

* **400 Bad Request**

```json
{ "error": ["Missing data or error information"] }
```

* **500 Internal Server Error**

```json
{ "error": "Internal Server Error" }
```

---

