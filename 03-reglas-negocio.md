# 📋 Reglas de Negocio - Sistema de Gestión de Glamping

Este documento define las políticas operacionales, restricciones lógicas y automatizaciones que gobiernan el funcionamiento del sistema de información del Glamping. Estas reglas garantizan la integridad de los datos y la consistencia de los procesos de negocio.

---

## 1. Gestión de Unidades Habitacionales (Domos)
* **RN-01 (Estados del Domo):** Un domo físico solo puede encontrarse en uno de tres estados operativos: `DISPONIBLE`, `OCUPADO` o `MANTENIMIENTO`. El estado por defecto al crear un domo es `DISPONIBLE`.
* **RN-02 (Automatización de Ocupación):** En el momento exacto en que se registre una nueva reserva en el sistema, el estado del domo asignado debe cambiar automáticamente a `OCUPADO`. *(Implementado mediante el Trigger: `trg_reserva_ocupa_domo`)*.
* **RN-03 (Liberación de Domos):** Al realizarse el proceso de salida del cliente, el domo debe quedar inmediatamente en estado `DISPONIBLE` para permitir nuevas contrataciones. *(Implementado mediante el Procedure: `sp_registrar_checkout`)*.

## 2. Control de Reservas y Clientes
* **RN-04 (Identificación Única):** Todo huésped debe registrarse obligatoriamente con su nombre, apellido y un documento de identidad (cédula o pasaporte). No pueden existir dos huéspedes con el mismo número de documento en el sistema.
* **RN-05 (Consistencia Cronológica):** La fecha de finalización (Check-Out) de una reserva debe ser estrictamente posterior a la fecha de inicio (Check-In). El sistema rechazará cualquier intento de registro que viole esta condición. *(Implementado mediante la Restricción: `chk_fechas`)*.

## 3. Políticas Financieras y de Precios
* **RN-06 (Cálculo Automatizado de Estadía):** El costo total de una estadía se calcula multiplicando el número total de noches programadas (Fecha Fin menos Fecha Inicio) por la tarifa vigente por noche correspondiente a la categoría del domo. *(Implementado mediante la Función: `fn_calcular_total_estadia`)*.
* **RN-07 (Control de Tarifas Recreativas):** El precio total de un servicio de aventura contratado equivale al precio unitario por persona de la actividad multiplicado por la cantidad de huéspedes registrados para asistir. Este valor debe ser mayor a cero.
* **RN-08 (Medios de Pago Permitidos):** El glamping liquida sus cuentas únicamente bajo tres modalidades financieras legalmente aceptadas: `EFECTIVO`, `TARJETA` o `TRANSFERENCIA`. Si un pago no se ha registrado, el estado financiero de la reserva se mantendrá como `PENDIENTE`.
