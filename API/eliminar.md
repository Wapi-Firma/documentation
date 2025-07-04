---
title: Eliminar Documento
layout: home
parent: API
nav_order: 2
description: "Elimina un documento no firmado."
---
Elimina un documento no firmado. 

# 🗑️ Eliminar Documento

**Endpoint:** `/api/deleteDocument/{id_seguimiento}`  
**Método:** `DELETE`  

## Parámetros
- `id_seguimiento` (string, requerido): ID del documento a eliminar.

## Headers

```
x-api-key: your-api-key-here
Content-Type: application/json
```

## Respuestas

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
