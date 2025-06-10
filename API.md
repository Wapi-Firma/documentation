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

```json
[
  {
    "id_seguimiento": "a55d68a337d91db31d",
    "uploaded_pdf": "...",
    "is_finished": false,
    "cantidad_firmantes": 2,
    ...
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

```json
{
  "file_url": "...",
  "is_finished": true,
  "signers": [
    {
      "name": "Luis Lacoste",
      "dni": "41595469",
      "signature": {
        "coords": "-32.123456,...",
        "hash": "SHA2-512...",
        ...
      }
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