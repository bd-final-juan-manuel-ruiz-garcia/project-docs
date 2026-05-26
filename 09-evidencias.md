# 📸 Evidencias de Ejecución y Pruebas del Sistema

Este documento recopila las pruebas de funcionamiento y consultas avanzadas ejecutadas directamente en el motor de base de datos PostgreSQL, demostrando el correcto comportamiento del diseño físico y la integridad de las relaciones.

---

## 1. Validación de Volumetría (Conteo de Registros)
Para comprobar que la inyección masiva de datos a través de Liquibase se ejecutó de forma correcta, se realiza un conteo general sobre la tabla central de operaciones:

```sql
hotel_db=# SELECT COUNT(*) FROM reserva;
 count 
-------
    45
(1 row)
SELECT 
    h.documento AS "Doc. Huésped",
    h.nombre || ' ' || h.apellido AS "Nombre Huésped",
    d.nombre_domo AS "Domo",
    td.nombre_tipo AS "Categoría", -- <-- Aquí entra la 7ma tabla (tipo_domo)
    r.fecha_inicio AS "Check-In",
    r.precio_total_estadia AS "Costo Estancia",
    COALESCE(aa.nombre_actividad, 'Ninguna') AS "Actividad",
    COALESCE(ra.precio_calculado, 0) AS "Costo Actividad",
    COALESCE(p.monto_pagado, 0) AS "Monto Pagado",
    COALESCE(p.metodo_pago, 'PENDIENTE') AS "Medio Pago"
FROM huesped h                                                -- 1. Tabla Huesped
JOIN reserva r ON h.id_huesped = r.id_huesped                 -- 2. Tabla Reserva
JOIN domo d ON r.id_domo = d.id_domo                           -- 3. Tabla Domo
JOIN tipo_domo td ON d.id_tipo_domo = td.id_tipo_domo         -- 4. Tabla Tipo_Domo
LEFT JOIN registro_actividad ra ON r.id_reserva = ra.id_reserva -- 5. Tabla Registro_Actividad
LEFT JOIN actividad_aventure aa ON ra.id_actividad = aa.id_actividad -- 6. Tabla Actividad_Aventure
LEFT JOIN pago p ON r.id_reserva = p.id_reserva               -- 7. Tabla Pago
LIMIT 12;

Doc. Huésped | Nombre Huésped |     Domo      |        Categoría         |  Check-In  | Costo Estancia |          Actividad           | Costo Actividad | Monto Pagado |  Medio Pago   
--------------+----------------+---------------+--------------------------+------------+----------------+------------------------------+-----------------+--------------+---------------
 1020456789   | Ana Gomez      | Domo Luna     | Estándar Familiar        | 2026-05-27 |      450000.00 | Canopy sobre el Bosque       |        70000.00 |    450000.00 | TARJETA
 1032789123   | Luis Rodriguez | Domo Estrella | Premium Vista Panorámica | 2026-05-29 |      450000.00 | Cabalgata Guiada             |        70000.00 |    450000.00 | TRANSFERENCIA
 1075612345   | Maria Martinez | Domo Galaxia  | Premium Vista Panorámica | 2026-05-31 |      450000.00 | Tour de Avistamiento de Aves |        70000.00 |    450000.00 | EFECTIVO
 1088456123   | Jorge Lopez    | Domo Eclipse  | VIP con Jacuzzi Privado  | 2026-06-02 |      450000.00 | Senderismo Nocturno          |        70000.00 |    450000.00 | TARJETA
 1019234568   | Diana Perez    | Domo Sol      | Estándar Familiar        | 2026-06-04 |      450000.00 | Canopy sobre el Bosque       |        70000.00 |    450000.00 | TRANSFERENCIA
 1021456790   | Pedro Sanchez  | Domo Luna     | Estándar Familiar        | 2026-06-06 |      450000.00 | Cabalgata Guiada             |        70000.00 |    450000.00 | EFECTIVO
 1033789124   | Lucia Ramirez  | Domo Estrella | Premium Vista Panorámica | 2026-06-08 |      450000.00 | Tour de Avistamiento de Aves |        70000.00 |    450000.00 | TARJETA
 1076612346   | Andres Torres  | Domo Galaxia  | Premium Vista Panorámica | 2026-06-10 |      450000.00 | Senderismo Nocturno          |        70000.00 |    450000.00 | TRANSFERENCIA
 1089456124   | Elena Castro   | Domo Eclipse  | VIP con Jacuzzi Privado  | 2026-06-12 |      450000.00 | Canopy sobre el Bosque       |        70000.00 |    450000.00 | EFECTIVO
 1015234569   | Sergio Herrera | Domo Sol      | Estándar Familiar        | 2026-06-14 |      450000.00 | Cabalgata Guiada             |        70000.00 |    450000.00 | TARJETA
 1022456791   | Martha Rojas   | Domo Luna     | Estándar Familiar        | 2026-06-16 |      450000.00 | Tour de Avistamiento de Aves |        70000.00 |    450000.00 | TRANSFERENCIA
 1034789125   | Fabian Ortiz   | Domo Estrella | Premium Vista Panorámica | 2026-06-18 |      450000.00 | Senderismo Nocturno          |        70000.00 |    450000.00 | EFECTIVO
(12 rows)
