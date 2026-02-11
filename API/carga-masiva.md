---
title: Carga Masiva
layout: home
parent: API
nav_order: 2
description: "Envío masivo de documentos para firma electrónica vía WhatsApp."
---

# 🗂️ Carga Masiva de Documentos

Permite el envío **masivo de documentos PDF o DOCX** para **firma electrónica vía WhatsApp**, con soporte para reemplazo de variables, firma electrónica legal y flujos con o sin validación de fotos.

**Endpoint:** `/sendCargaMasiva`  
**Método:** `POST`

## Attributes

- ### Document Settings - Object
    - **document_type (string, required)**  
      Tipo de documento a enviar.  
      Valores permitidos: `pdf` | `docx`
    - **document (string, required)** - La URL pública o el documento en Base64 que se va a firmar.
    - **request_photos (boolean, optional)** - Solicita fotos durante el flujo de firma. El valor por defecto es `false`.
    - **validate_photos (boolean, optional)** - Si es true, se validará el DNI del firmante durante el proceso de firma. El valor por defecto es `true`.
        > ⚠️ Disponible solo para usuarios del plan **PRO**.

- ### firma - Object (optional)

    Define la posición de la firma dentro del documento.
    - **page (integer, optional)** - Número de página donde se aplica la firma (comienza en 1).
    - **position_x (integer, optional)** - Coordenada X de la firma (en píxeles).
    - **position_y (integer, optional)** - Coordenada Y de la firma (en píxeles).

- ### df (array[], required) - El arreglo de firmantes.

    Cada objeto consume **1 crédito** y contiene:
    - **id (string, optional)** - Identificador interno del firmante.
    - **name (string, required)** - Nombre y apellido del firmante.
    - **dni (string, required)** - DNI del firmante.
    - **whatsapp (string, required)** - Número de WhatsApp del firmante (formato internacional).

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

## Headers

```
x-api-key: your-api-key-here
Content-Type: application/json
```

## Body (ejemplo)

```json
{
    "document_type": "pdf",
    "document": "<base64-del-documento>",
    "request_photos": true,
    "validate_photos": true,
    "df": [
        {
            "id": 123,
            "name": "Juan Pérez",
            "dni": "12345678",
            "whatsapp": "5491122334455"
        },
        {
            "id": 456,
            "name": "María Gómez",
            "dni": "87654321",
            "whatsapp": "5491166778899"
        }
    ],
    "firmante": {
        "page": 1,
        "position_x": 120,
        "position_y": 450
    },
    "webhook_url": "https://noApiKey.com",
    "webhook_key": { "key": "value" },
}
```

## Respuesta exitosa

```json
[
    {
        "link": "https://midominio.wapifirma.com/rd/123",
        "name": "Juan Pérez",
        "dni": "12345678",
        "contact": "5491122334455",
        "method": "wpp",
        "id_custom": 123
    }
]
```

## Respuestas de error

- **400 Bad Request**

```json
{ "error": "Missing fields" }
```

- **403 Forbidden**

```json
{ "error": "API Key bloqueada" }
```

- **429 Too Many Requests**

```json
{ "error": "Sin créditos disponibles" }
```

---
