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
