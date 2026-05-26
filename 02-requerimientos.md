# 📋 Especificación de Requerimientos - Sistema de Glamping

Este documento detalla las necesidades funcionales y restricciones técnicas que el motor de base de datos relacional debe satisfacer para garantizar la correcta operación del Glamping.

---

## 1. Requerimientos Funcionales (RF)
Los requerimientos funcionales definen los servicios operacionales y las transacciones que el sistema debe ejecutar de forma obligatoria.

* **RF-01: Gestión de Huéspedes:** El sistema debe registrar, actualizar y almacenar los datos de identificación básicos de los clientes (Nombres, Apellidos y Documento Único).
* **RF-02: Control de Inventario Habitacional:** El sistema debe permitir la clasificación de los domos por tipos o categorías, asignando a cada una un precio base por noche y controlando el estado de disponibilidad físico en tiempo real.
* **RF-03: Automatización de Reservas:** El sistema debe permitir la creación de reservas vinculando un cliente, un domo y un rango de fechas válido, calculando de forma automática el costo total de la estadía mediante lógica interna.
* **RF-04: Liquidación de Servicios Adicionales:** El sistema debe permitir asociar múltiples actividades de aventura del catálogo a una reserva específica, multiplicando el costo por persona por la cantidad de asistentes para generar el cobro.
* **RF-05: Registro Financiero de Pagos:** El sistema debe capturar cada transacción monetaria en caja asociada a una reserva, validando que se use un medio de pago permitido (Efectivo, Tarjeta, Transferencia) y guardando la fecha y hora exacta del recaudo.
* **RF-06: Liberación Operativa (Check-Out):** El sistema debe proveer un mecanismo explícito para cerrar la estadía de una reserva y cambiar de manera inmediata el estado del domo de 'OCUPADO' a 'DISPONIBLE'.

---

## 2. Requerimientos No Funcionales (RNF)
Los requerimientos no funcionales definen las restricciones técnicas, de rendimiento, seguridad y calidad bajo las cuales opera el sistema.

* **RNF-01: Persistencia y Motor de Datos:** El sistema debe utilizar el motor de base de datos relacional **PostgreSQL versión 15** o superior, asegurando soporte completo para transacciones ACID.
* **RNF-02: Portabilidad y Despliegue:** La infraestructura de la base de datos debe estar completamente contenerizada utilizando **Docker** y administrada mediante **Docker Compose**, permitiendo su despliegue inmediato en cualquier sistema operativo.
* **RNF-03: Control de Versiones del Esquema:** Todos los cambios en la estructura física (DDL) y la inserción de datos iniciales (DML) deben gestionarse de forma automatizada mediante **Liquibase**, evitando la ejecución manual de scripts sueltos.
* **RNF-04: Integridad Referencial y Restricciones:** El diseño físico debe implementar llaves primarias (`PRIMARY KEY`), llaves foráneas (`FOREIGN KEY`) con borrado/actualización en cascada cuando corresponda, y restricciones de verificación (`CHECK`) para evitar datos corruptos (ej. precios negativos o fechas incoherentes).
* **RNF-05: Trazabilidad Temporal:** El registro de las transacciones financieras debe almacenar marcas de tiempo precisas (`TIMESTAMP DEFAULT CURRENT_TIMESTAMP`) generadas directamente por el servidor de la base de datos.
