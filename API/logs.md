---
title: Registro de eventos
layout: home
parent: API
nav_order: 3
description: "Obtiene la cronología de eventos relacionados con un seguimiento (firmantes, entregas, validaciones biométricas, rechazos, etc.)."
---

# 📜 Event Logs

Devuelve la lista cronológica de eventos asociados a un `id_seguimiento`. Incluye información sobre entregas por WhatsApp, respuestas de plantilla, subidas de imágenes (selfie/DNI), validaciones biométricas, firma del documento y rechazos.

**Endpoint:** `/api/eventLogs/:id_seguimiento`  
**Método:** `GET`

## Headers

```
Content-Type: application/json
x-api-key: your-api-key-here
```

## Parámetros

- `:id_seguimiento` (path, string, requerido): identificador del seguimiento/documento cuya cronología se desea consultar.

## Respuesta (200)

La respuesta contiene un objeto con metadatos del seguimiento y un arreglo `events` con los eventos ordenados cronológicamente.

```json
{
  "created_at": "2024-09-12T10:00:00.000Z",
  "autenticacion": "Foto selfie + Foto ID/DNI (sin validación)",
  "id_seguimiento": "d4dc281aceb48e11fa",
  "events": [
    {
      "event": "whatsapp_delivered",
      "signer": "Juan Pérez",
      "phone": "+5491112345678",
      "timestamp": "2024-09-12T10:01:00.000Z"
    },
    {
      "event": "signature_link_sent",
      "signer": "Juan Pérez",
      "phone": "+5491112345678",
      "timestamp": "2024-09-12T10:02:00.000Z"
    },
    {
      "event": "selfie_biometric_validation",
      "signer": "Juan Pérez",
      "phone": "+5491112345678",
      "attempt": 1,
      "result": "approved",
      "timestamp": "2024-09-12T10:03:00.000Z",
      "link": "https://wapi-img.s3.us-east-2.amazonaws.com/12345sf.jpeg"
    },
    {
      "event": "mrz_validation",
      "signer": "Juan Pérez",
      "phone": "+5491112345678",
      "timestamp": "2024-09-12T10:04:57.000Z"
    },
    {
      "event": "document_signed",
      "signer": "Juan Pérez",
      "phone": "+5491112345678",
      "timestamp": "2024-09-12T10:05:00.000Z"
    }
  ]
}
```

## Campos devueltos

- `created_at` (string): fecha de creación del seguimiento.
- `autenticacion` (string): tipo de autenticación utilizada para el flujo.
- `id_seguimiento` (string): identificador del seguimiento consultado.
- `events` (array): arreglo de eventos ordenados cronológicamente. Cada elemento puede contener campos comunes y campos específicos según el tipo de evento.

# 📌 Eventos y significado

- **whatsapp_delivered**: mensaje entregado al firmante
- **template_response**: el firmante respondió (ej: “Revisar y firmar”)
- **no_continue**: el firmante decidió no continuar
- **signature_link_sent**: se envió el link de firma
- **document_signed**: el documento fue firmado
- **dni_error**: error relacionado al documento/DNI
- **selfie_image_uploaded / dni_image_uploaded**: imagen subida (sin validar)
- **selfie_biometric_validation / dni_biometric_validation**: resultado biométrico (aprobado/rechazado + intento)
- **mrz_validation**: validación del documento previa a la firma
- **edatalia_failed_validation / edatalia_rejection / document_rejected**: rechazo o fallo en validaciones externas o del proceso

## Ejemplo de petición (curl)

```bash
curl -X GET "https://api.tudominio.com/api/eventLogs/d4dc281aceb48e11fa" \
	-H "Content-Type: application/json" \
	-H "x-api-key: REEMPLAZAR_POR_TU_API_KEY"
```

## Códigos de respuesta

- `200 OK` — Devuelve la cronología de eventos.
- `404 Not Found` — `{"error":"Document not found"}` cuando no existe el `id_seguimiento`.
- `500 Internal Server Error` — Error inesperado del servidor.
