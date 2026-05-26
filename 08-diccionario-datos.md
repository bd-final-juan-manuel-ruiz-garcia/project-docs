# 📖 Diccionario de Datos Físico - Sistema de Gestión de Glamping

Este documento detalla la estructura física, tipos de datos y restricciones aplicadas a las tablas del motor de base de datos PostgreSQL para el proyecto de gestión del Glamping.

---

## 1. Tabla: `tipo_domo`
Almacena las categorías de hospedaje disponibles en el glamping y sus tarifas base por noche.

| Columna | Tipo de Dato | Restricciones / Atributos | Descripción |
| :--- | :--- | :--- | :--- |
| `id_tipo_domo` | SERIAL | PRIMARY KEY, NOT NULL | Identificador único autoincremental de la categoría. |
| `nombre_tipo` | VARCHAR(50) | NOT NULL | Nombre comercial de la categoría (ej. VIP con Jacuzzi, Estándar Familiar). |
| `precio_por_noche` | NUMERIC(10,2) | NOT NULL, CHECK (> 0) | Costo base por noche de estadía en esa categoría de domo. |

## 2. Tabla: `domo`
Representa las unidades habitacionales físicas e individuales del Glamping.

| Columna | Tipo de Dato | Restricciones / Atributos | Descripción |
| :--- | :--- | :--- | :--- |
| `id_domo` | SERIAL | PRIMARY KEY, NOT NULL | Identificador único del domo físico. |
| `nombre_domo` | VARCHAR(50) | NOT NULL | Nombre propio asignado al domo (ej. Domo Sol, Domo Luna). |
| `id_tipo_domo` | INT | FOREIGN KEY REFERENCES `tipo_domo` | Vinculación con la categoría de tarifa correspondiente. |
| `estado` | VARCHAR(20) | DEFAULT 'DISPONIBLE', CHECK | Estado operativo. Valores válidos: 'DISPONIBLE', 'OCUPADO', 'MANTENIMIENTO'. |

## 3. Tabla: `huesped`
Registra los datos de identificación obligatorios de los clientes del glamping.

| Columna | Tipo de Dato | Restricciones / Atributos | Descripción |
| :--- | :--- | :--- | :--- |
| `id_huesped` | SERIAL | PRIMARY KEY, NOT NULL | Identificador interno único del cliente en el sistema. |
| `nombre` | VARCHAR(50) | NOT NULL | Nombre de pila del huésped. |
| `apellido` | VARCHAR(50) | NOT NULL | Apellido del huésped. |
| `documento` | VARCHAR(20) | UNIQUE, NOT NULL | Cédula, pasaporte o documento de identidad. No admite duplicados. |

## 4. Tabla: `reserva`
Entidad transaccional central que registra el alquiler de un domo por parte de un huésped durante un rango de fechas.

| Columna | Tipo de Dato | Restricciones / Atributos | Descripción |
| :--- | :--- | :--- | :--- |
| `id_reserva` | SERIAL | PRIMARY KEY, NOT NULL | Número único e inequívoco de la reserva. |
| `id_huesped` | INT | FOREIGN KEY REFERENCES `huesped` | Identificador del cliente que contrató el hospedaje. |
| `id_domo` | INT | FOREIGN KEY REFERENCES `domo` | Identificador del domo físico asignado. |
| `fecha_inicio` | DATE | NOT NULL | Fecha pactada para el ingreso del cliente (Check-In). |
| `fecha_fin` | DATE | NOT NULL | Fecha pactada para la salida del cliente (Check-Out). |
| `precio_total_estadia` | NUMERIC(10,2) | NOT NULL, CHECK (>= 0) | Valor total calculado por las noches de hospedaje. |

* **Restricción de Integridad de Fechas (`chk_fechas`):** `CHECK (fecha_fin > fecha_inicio)`. Garantiza lógicamente que la salida sea posterior a la entrada.

## 5. Tabla: `actividad_aventure`
Catálogo de los servicios turísticos y de aventura adicionales ofrecidos en el destino.

| Columna | Tipo de Dato | Restricciones / Atributos | Descripción |
| :--- | :--- | :--- | :--- |
| `id_actividad` | SERIAL | PRIMARY KEY, NOT NULL | Identificador único de la actividad recreativa. |
| `nombre_actividad` | VARCHAR(100) | NOT NULL | Nombre comercial del servicio (ej. Canopy, Senderismo Nocturno). |
| `precio_persona` | NUMERIC(10,2) | NOT NULL, CHECK (>= 0) | Costo unitario cobrado por cada participante. |

## 6. Tabla: `registro_actividad`
Entidad intermedia operacional que rompe la relación de muchos a muchos entre las reservas y los servicios de aventura.

| Columna | Tipo de Dato | Restricciones / Atributos | Descripción |
| :--- | :--- | :--- | :--- |
| `id_registro` | SERIAL | PRIMARY KEY, NOT NULL | Identificador único de consumo de servicios de la reserva. |
| `id_reserva` | INT | FOREIGN KEY REFERENCES `reserva` | Reserva asociada que contrató la actividad. |
| `id_actividad` | INT | FOREIGN KEY REFERENCES `actividad_aventure` | Servicio específico seleccionado del catálogo. |
| `cantidad_personas` | INT | NOT NULL, CHECK (> 0) | Número de pasajeros que asistirán a la actividad. |
| `precio_calculado` | NUMERIC(10,2) | NOT NULL | Subtotal monetario liquidado por este consumo específico. |

## 7. Tabla: `pago`
Registra las transacciones financieras y pasarelas de pago asociadas al recaudo monetario de las reservas.

| Columna | Tipo de Dato | Restricciones / Atributos | Descripción |
| :--- | :--- | :--- | :--- |
| `id_pago` | SERIAL | PRIMARY KEY, NOT NULL | Número de recibo físico de caja o transacción digital. |
| `id_reserva` | INT | FOREIGN KEY REFERENCES `reserva` | Reserva de la cual se está cobrando el dinero. |
| `monto_pagado` | NUMERIC(10,2) | NOT NULL, CHECK (> 0) | Cantidad exacta de dinero recaudada en la transacción. |
| `metodo_pago` | VARCHAR(30) | CHECK | Medio utilizado. Valores permitidos: 'EFECTIVO', 'TARJETA', 'TRANSFERENCIA'. |
| `estado_pago` | VARCHAR(20) | DEFAULT 'COMPLETADO' | Situación del recaudo financiero en el balance. |
| `fecha_pago` | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | Sello de tiempo exacto (fecha y hora) en que ingresó el dinero. |
