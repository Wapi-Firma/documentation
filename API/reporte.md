---
title: Reporte
layout: home
parent: API
nav_order: 5
description: "Devuelve el reporte legal del documento firmado en formato PDF binario."
---

Devuelve el reporte legal del documento firmado en formato PDF binario.

# 📄 Descargar Reporte PDF

**Endpoint:** `/api/reporte/{id_seguimiento}`
**Método:** `GET`

## Parámetro

- **`id_seguimiento` (string, requerido)**

## Respuestas

- **200 OK:** Devuelve PDF binario.
- **404 Not Found:**

```json
{ "error": "Documento no encontrado" }
```