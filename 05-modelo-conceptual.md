# 🗺️ Modelo Conceptual del Sistema

Diagrama conceptual de alto nivel que identifica las entidades principales del Glamping y sus relaciones de negocio.

```mermaid
erDiagram
    HUESPED ||--o{ RESERVA : "realiza"
    DOMO ||--o{ RESERVA : "es_asignado"
    TIPO-DOMO ||--o{ DOMO : "clasifica"
    RESERVA ||--o{ PAGO : "registra"
    RESERVA ||--o{ REGISTRO-ACTIVIDAD : "contrata"
    ACTIVIDAD-AVENTURE ||--o{ REGISTRO-ACTIVIDAD : "incluye"
