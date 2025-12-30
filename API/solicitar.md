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
  - **campaign_id (integer, optional)**  - El ID de la campaña (plantilla) asociada al documento. Para usar este campo, primero debés [crear una plantilla de firma]({% link crear-plantilla.md %}) en la app. Luego, contactá al equipo de Wapi para solicitar el ID de campaña vinculado a esa plantilla.
  - **document (string, required)**  - La URL pública o el documento en Base64 que se va a firmar.
  - **firma_en_todas_las_hojas (boolean, optional)**  - Si es `true`, la firma se aplicará en todas las páginas del documento. El valor por defecto es `False`.
  - **document_name (string, optional)**  - El nombre del documento. El valor por defecto es `Wapi firma document`.
  - **request_photos (boolean, optional)**  - Solicita fotos durante el flujo de firma. El valor por defecto es `False`.
  - **validate_photos (boolean, optional)**  - Si es `true`, se validará el DNI del firmante durante el proceso de firma. El valor por defecto es `True`. (Nota: Esta función solo está disponible para usuarios del plan Wapi Pro.)
  - **custom_parameters (object, optional)**  -  Un campo flexible para metadatos personalizados relacionados con el documento. El valor por defecto es `null`.
- ### Communication - Object
  - **webhook_url (string, optional):** La URL del webhook para enviar una notificación cuando el proceso de firma esté completo.
    - **Tipos de webhooks:** Hay tres tipos de notificaciones webhook:
      - **alert** - Te avisa que ya se terminó de firmar el documento
      - **report** - Te avisa que se terminó de generar el reporte
      - **whatsapp** - Te avisa que no se pudo entregar un documento al WhatsApp del firmante
    - Body: La estructura del payload del callback.
      - Ejemplo (tipo alert)
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
  - **webhook_key (object, optional):** El par clave-valor para la URL del webhook, si es necesario.
      - Ejemplo
        ```json
        {
          "key": "value"
        }
        ```
  - **wpp (boolean, optional):** Indica si se debe enviar una notificación de WhatsApp al firmante. El valor por defecto es `True`.

### datos_firmantes (array[], required) - El arreglo de firmantes. Cada objeto contiene:
+ name (string, required) - El nombre del firmante.
+ dni (string, required) - El DNI del firmante.
+ phone (string, required) - El número de teléfono del firmante.
+ position (integer, optional) -  El orden o posición de firma dentro del documento (se usa para determinar la ubicación).
+ position_x (integer, optional) - La coordenada X para la ubicación de la firma (en píxeles).
+ position_y (integer, optional) - La coordenada Y para la ubicación de la firma (en píxeles).
+ page (integer, optional) - El número de página donde se debe colocar la firma (comenzando desde 1).
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
  "validate_photos": true,
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
      "link": "https://firma.wapifirma.com/rd/F5384BECE0574D809CC129F90C90253B",
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

