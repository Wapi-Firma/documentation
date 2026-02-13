---
title: Carga Masiva
layout: home
parent: API
nav_order: 2
description: "Envío masivo de documentos para firma electrónica vía WhatsApp."
---

# 🗂️ Carga Masiva de Documentos

Este endpoint permite el **envío masivo de documentos PDF o DOCX** para **firma electrónica vía WhatsApp**, con soporte para:

- Reemplazo dinámico de variables por firmante
- Firma electrónica con validez legal
- Flujos con o sin validación de fotos / DNI

**Endpoint:** `/sendCargaMasiva`  
**Método:** `POST`

---

## Attributes

### 📄 Document Settings — Object

- **document_type** `(string, required)`  
  Tipo de documento a enviar.  
  Valores permitidos: `pdf` | `docx`

- **document** `(string, required)`  
  URL pública del documento o contenido en Base64 que se va a firmar.

- **request_photos** `(boolean, optional)`  
  Solicita fotos durante el flujo de firma.  
  Default: `false`

- **validate_photos** `(boolean, optional)`  
  Si es `true`, se validará el DNI del firmante durante el proceso.  
  Default: `true`  
  > ⚠️ Disponible solo para usuarios del plan **PRO**.

---

### firma — Object (optional)

Define la posición visual de la firma dentro del documento.

- **page** `(integer)` — Página donde se aplica la firma (comienza en 1)
- **position_x** `(integer)` — Coordenada X en píxeles
- **position_y** `(integer)` — Coordenada Y en píxeles

---

### df — Array<Object> (required)

Arreglo de firmantes.  
Cada objeto **consume 1 crédito**.

Campos obligatorios por firmante:

- **id** `(string, optional)` — Identificador interno
- **nombre y apellido** `(string, required)`
- **dni** `(string, required)`
- **whatsapp** `(string, required)` — Formato internacional

---

## 🔁 Reemplazo de Variables por Firmante

Cada objeto dentro de `df` puede incluir **variables personalizadas** en formato **clave–valor**.  
Estas variables se usan para **reemplazar automáticamente** contenidos del documento (PDF o DOCX).

### ¿Cómo funciona?

- En el documento, las variables deben escribirse **entre llaves**:

  ```
  {nombre_variable}
  ```

- En el objeto del firmante (`df`), se envía la variable con el mismo nombre.
- Cada firmante recibe su **documento personalizado**.

### Ejemplo

#### Documento (PDF / DOCX)

```
Estado civil: {estado_civil}
Número de póliza: {poliza}
```

#### Body — df

```json
{
  "df": [
    {
      "id": 123,
      "nombre y apellido": "Juan Pérez",
      "dni": "12345678",
      "whatsapp": "5491122334455",
      "estado_civil": "soltero",
      "poliza": "A-210"
    },
    {
      "id": 456,
      "nombre y apellido": "María Gómez",
      "dni": "87654321",
      "whatsapp": "5491166778899",
      "estado_civil": "casada",
      "poliza": "B-330"
    }
  ]
}
```

#### Resultado

- Juan recibe:
  ```
  Estado civil: soltero
  Número de póliza: A-210
  ```

- María recibe:
  ```
  Estado civil: casada
  Número de póliza: B-330
  ```

### ⚠️ Consideraciones

- No hay límite de variables por firmante.
- Los nombres deben coincidir **exactamente** (case-sensitive).
- Si una variable existe en el documento pero no en el `df`, **no se reemplaza**.

---

### Communication — Object

- **webhook_url** `(string, optional)`  
  URL para recibir notificaciones del proceso.

  Tipos:
  - `alert` — Documento firmado
  - `report` — Reporte generado
  - `whatsapp` — Error de entrega por WhatsApp

- **webhook_key** `(object, optional)`  
  Par clave–valor adicional para el webhook.

- **wpp** `(boolean, optional)`  
  Envía notificación por WhatsApp al firmante.  
  Default: `true`

---

## Headers

```
x-api-key: your-api-key-here
Content-Type: application/json
```

---

## Body (ejemplo completo)

```json
{
  "document_type": "pdf",
  "document": "<base64-del-documento>",
  "request_photos": true,
  "validate_photos": true,
  "df": [
    {
      "id": 123,
      "nombre y apellido": "Juan Pérez",
      "dni": "12345678",
      "whatsapp": "5491122334455",
      "estado_civil": "soltero"
    }
  ],
  "firmante": {
    "page": 1,
    "position_x": 120,
    "position_y": 450
  },
  "webhook_url": "https://noApiKey.com",
  "webhook_key": { "key": "value" }
}
```

---

## ✅ Respuesta exitosa

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

---

## ❌ Respuestas de error

### 400 — Bad Request
```json
{ "error": "Missing fields" }
```

### 403 — Forbidden
```json
{ "error": "API Key bloqueada" }
```

### 429 — Too Many Requests
```json
{ "error": "Sin créditos disponibles" }
```
