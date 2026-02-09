---
title: Manejo de Equipo
layout: home
nav_order: 4
description: "Cómo funcionan los roles, permisos y el uso de firmas en WapiFirma"
permalink: /manejo-equipos
---

# Manejo de Equipos

Este documento explica cómo funcionan los **equipos en WapiFirma**, detallando los **roles**, **permisos** y la administración de **firmas y documentos** dentro de una cuenta.

---

## Roles del equipo

En WapiFirma existen **tres roles principales**: **Owner**, **Admin** y **User**.  
Cada uno tiene distintos permisos y responsabilidades.

---

## Owner (Propietario)

El **Owner** es el responsable principal de la cuenta.

- Es el **dueño del balance de firmas**, los documentos y las firmas enviadas.
- Todas las firmas compradas por Admins se **suman al balance del Owner**.
- Todas las firmas utilizadas por Admins o Users se **descuentan del balance del Owner**.
- Ve **todos los documentos** enviados por cualquier miembro del equipo.
- Es quien **crea el equipo** inicialmente e invita a los primeros miembros.
- **No puede ser eliminado ni expulsado** del equipo.
- Solo puede existir **un único Owner por cuenta** y **el rol no es transferible**.

---

## Admin

El **Admin** colabora en la gestión del equipo y la cuenta.

- Es invitado por el Owner o promovido desde un rol User.
- Puede:
  - Ver todos los documentos del equipo.
  - Acceder al billing.
  - Comprar firmas (las firmas se agregan al balance del Owner).
  - Crear etiquetas.
  - Cambiar el rol de Users a Admin.
  - Invitar nuevos usuarios al equipo.

---

## User

El **User** tiene permisos limitados.

- Solo puede ver los **documentos que él mismo envía**.
- No puede:
  - Invitar nuevos usuarios.
  - Cambiar roles.
  - Crear etiquetas.
  - Acceder al billing ni comprar firmas.
  - Puede enviar documentos para firmar.

---

## Reglas importantes del sistema

### Balance de firmas
- El balance de firmas es **centralizado en el Owner**.
- Las compras de firmas hechas por Admins **suman** al balance del Owner.
- El consumo de firmas por cualquier miembro **descuenta** del balance del Owner.

---

### Visibilidad de documentos
- **Owners y Admins**: ven todos los documentos enviados por el equipo.
- **Users**: solo ven los documentos que ellos mismos enviaron.

---

### Owner
- No puede ser eliminado ni expulsado.
- Solo existe un Owner por equipo.
- El rol de Owner **no se puede transferir**.

---

### Invitaciones al equipo
- El Owner crea el equipo e invita a los miembros.
- Los Admins pueden invitar.
- La invitación se envía por correo electrónico.
  - Si no llega, revisar la carpeta de spam.

---

## Casos comunes

- **Un Admin compra firmas**  
  Las firmas se suman al balance del Owner.

- **Un User envía documentos a firmar**  
  Se descuentan firmas del balance del Owner y el User solo ve su historial.

- **Un User es promovido a Admin**  
  Obtiene permisos administrativos y mayor visibilidad.

---

## Recomendaciones

- Mantener al Owner como cuenta principal de facturación.
