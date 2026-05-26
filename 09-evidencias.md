# 📸 Evidencias de Ejecución y Pruebas del Sistema

Este documento recopila las pruebas de funcionamiento y consultas avanzadas ejecutadas directamente en el motor de base de datos PostgreSQL, demostrando el correcto comportamiento del diseño físico y la integridad de las relaciones.

---

## 1. Validación de Infraestructura (Contenedores Activos)
Para demostrar el correcto despliegue del entorno contenerizado, se verifica el estado operativo de los servicios mediante Docker Compose:

[AQUÍ_IMAGEN_1]
<img width="1919" height="1079" alt="evidencia-docker" src="https://github.com/user-attachments/assets/9b6bfd48-5ff9-4c50-840b-838f38973c7d" />
---

## 2. Validación de Volumetría (Conteo de Registros)
Para comprobar que la inyección masiva de datos a través de Liquibase se ejecutó de forma correcta, se realiza un conteo general sobre la tabla central de operaciones:

sql
hotel_db=# SELECT COUNT(*) FROM reserva;
 count 
-------
    45
(1 row)

[AQUÍ_IMAGEN_2]
<img width="1919" height="1077" alt="evidencia-conteo" src="https://github.com/user-attachments/assets/a78e7ae9-f830-4afb-9e5b-332a7b8da78a" />

Consulta Avanzada de 7 Tablas Simultáneas (Reporte Gerencial)
Con el fin de consolidar la cuenta y la experiencia de los clientes, se diseñó una consulta compleja que realiza un cruce relacional de 7 tablas simultáneas utilizando uniones de tipo JOIN y funciones de control de nulos (COALESCE).
SELECT 
    
    h.documento AS "Doc. Huésped",
    h.nombre || ' ' || h.apellido AS "Nombre Huésped",
    d.nombre_domo AS "Domo",
    td.nombre_tipo AS "Categoría",
    r.fecha_inicio AS "Check-In",
    r.precio_total_estadia AS "Costo Estancia",
    COALESCE(aa.nombre_actividad, 'Ninguna') AS "Actividad",
    COALESCE(ra.precio_calculado, 0) AS "Costo Actividad",
    COALESCE(p.monto_pagado, 0) AS "Monto Pagado",
    COALESCE(p.metodo_pago, 'PENDIENTE') AS "Medio Pago" 
    FROM huesped h
    JOIN reserva r ON h.id_huesped = r.id_huesped
    JOIN domo d ON r.id_domo = d.id_domo
    JOIN tipo_domo td ON d.id_tipo_domo = td.id_tipo_domo
    LEFT JOIN registro_actividad ra ON r.id_reserva = ra.id_reserva
    LEFT JOIN actividad_aventure aa ON ra.id_actividad = aa.id_actividad
    LEFT JOIN pago p ON r.id_reserva = p.id_reserva
    LIMIT 12;

[AQUÍ_IMAGEN_3]
* **Integrante Responsable:** Juan Manuel Ruiz García
* **Objeto Evaluado:** Consulta con JOIN de más de 5 tablas (Involucra 7 tablas: huesped, reserva, domo, tipo_domo, registro_actividad, actividad_aventure, pago).
* **Propósito del Negocio:** Consolidar el reporte gerencial de cuentas por estancia de huéspedes, discriminando costos de alojamiento, consumos de actividades de aventura y estado de transacciones financieras.
* **Resultado Esperado:** Tabla relacional ordenada con un límite estricto de 12 registros de prueba transaccional.
<img width="1911" height="1077" alt="evidencia-reporte" src="https://github.com/user-attachments/assets/1decf907-229c-4fd1-b87a-a56f1e98aab4" />

---

## 4. Validación de Objetos Programados Libres (Por Integrante)
Para dar cumplimiento al alcance individual de objetos SQL en el servidor (Sección 7.6 de la guía), se detalla la ubicación de los elementos lógicos desarrollados:

* **Integrante Responsable:** Juan Manuel Ruiz García
* **Trigger Implementado:** `trg_actualizar_precio_actividad` (Controla el cálculo automático de tarifas en la tabla de registros de aventura).
  * *Ubicación del código:* `project-bd/liquibase/ddl/triggers/`
* **Procedure Implementado:** `sp_registrar_pago_completo` (Automatiza la inserción segura de transacciones financieras).
  * *Ubicación del código:* `project-bd/liquibase/ddl/procedures/`
* **Function Implementada:** `fn_calcular_dias_estancia` (Determina la diferencia de jornadas operacionales entre Check-In y Check-Out).
  * *Ubicación del código:* `project-bd/liquibase/ddl/functions/`

