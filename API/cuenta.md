---
title: Cuenta
layout: home
parent: API
nav_order: 6
description: "Actualiza el nombre comercial que verán los firmantes en WhatsApp."
---

# PUT /api/account/changeBusinessName

Actualiza el nombre comercial que verán los firmantes cuando reciban el mensaje de WhatsApp para firmar. Este valor se muestra al firmante como remitente/identificador de la organización que solicita la firma.

**Endpoint:** `/api/account/changeBusinessName`  
**Método:** `PUT`

## Headers

```
Content-Type: application/json
x-api-key: your-api-key-here
```

> Nota: Este endpoint requiere una `x-api-key` válida. La validación se realiza en el middleware `apiKeyValidation`.

## Body (ejemplo)

```json
{
  "business_name": "Mi Empresa S.A."
}
```

## Campos

- `business_name` (string, requerido): nombre comercial que se desea mostrar a los firmantes.

## Visibilidad para firmantes

El valor de `business_name` es el texto que verán las personas que reciben el mensaje de WhatsApp para firmar. Aparece como el remitente/identificador de la organización al solicitar la firma.

Ejemplo (cómo lo podría ver el firmante en WhatsApp):

> Soy WapIA, asistente virtual de Mi Empresa S.A. WapiFirma me pidió hacerle llegar un documento para que lo firme.

## Ejemplo de petición (curl)

```bash
curl -X PUT "https://api.tudominio.com/api/account/changeBusinessName" \
  -H "Content-Type: application/json" \
  -H "x-api-key: REEMPLAZAR_POR_TU_API_KEY" \
  -d '{"business_name":"Mi Empresa S.A."}'
```

## Respuesta exitosa (200)

```json
{
  "message": "Business name updated successfully",
  "business_name": "Mi Empresa S.A."
}
```

## Respuestas de error

- **400 Bad Request** (por ejemplo, si falta `business_name` o no cumple validaciones)

```json
{ "message": "Invalid request: business_name is required" }
```

- **401 Unauthorized**

```json
{ "message": "Unauthorized" }
```

- **404 Not Found**

```json
{ "message": "User not found" }
```

- **500 Internal Server Error**

```json
{ "message": "Internal server error" }
```
