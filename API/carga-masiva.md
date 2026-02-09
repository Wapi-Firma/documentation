---
title: Carga Masiva
layout: home
parent: API
nav_order: 1
description: "Envío masivo de documentos para firma electrónica vía WhatsApp."
---

# 📄 Carga Masiva de Documentos

Permite el envío **masivo de documentos PDF o DOCX** para **firma electrónica vía WhatsApp**, con soporte para reemplazo de variables, firma electrónica legal y flujos con o sin validación de fotos.

**Endpoint:** `/sendCargaMasiva`  
**Método:** `POST`

## Attributes

- ### Document Settings - Object
    - **document_type (string, required)**  
      Tipo de documento a enviar.  
      Valores permitidos: `pdf` | `docx`
    - **doc (string, required)** - Documento codificado en Base64.
    - **request_photos (boolean, optional)** - Solicita selfie + foto del documento durante el flujo de firma. El valor por defecto es `false`.
    - **validate_photos (boolean, optional)** - Valida automáticamente las fotos enviadas por el firmante. El valor por defecto es `true`.
        > ⚠️ Disponible solo para usuarios del plan **PRO**.

- ### firma - Object (optional)
    Define la posición de la firma dentro del documento.
    - **page (integer, optional)** - Número de página donde se aplica la firma (comienza en 1).
    - **position_x (integer, optional)** - Coordenada X de la firma (en píxeles).
    - **position_y (integer, optional)** - Coordenada Y de la firma (en píxeles).

### df (array[], required) - El arreglo de firmantes.

Cada objeto consume **1 crédito** y contiene:

- **name (string, required)** - Nombre y apellido del firmante.
- **dni (string, required)** - DNI del firmante.
- **whatsapp (string, required)** - Número de WhatsApp del firmante (formato internacional).
- **id_custom (string, optional)** - Identificador interno del firmante.

## Headers

```
x-api-key: your-api-key-here
Content-Type: application/json
```

## Body (ejemplo)

```json
{
    "document_type": "pdf",
    "doc": "<base64-del-documento>",
    "request_photos": true,
    "validate_photos": true,
    "df": [
        {
            "name": "Juan Pérez",
            "dni": "12345678",
            "whatsapp": "5491122334455",
            "id_custom": 123
        },
        {
            "name": "María Gómez",
            "dni": "87654321",
            "whatsapp": "5491166778899",
            "id_custom": 456
        }
    ],
    "firmante": {
        "page": 1,
        "position_x": 120,
        "position_y": 450
    }
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
