# 📊 Modelo Lógico de Datos

Estructura lógica relacional que define la integridad referencial y las dependencias entre tablas.

```mermaid
erDiagram
    TIPO_DOMO {
        int id_tipo_domo PK
        string nombre_tipo
        numeric precio_por_noche
    }
    DOMO {
        int id_domo PK
        string nombre_domo
        int id_tipo_domo FK
        string estado
    }
    HUESPED {
        int id_huesped PK
        string nombre
        string apellido
        string documento
    }
    RESERVA {
        int id_reserva PK
        int id_huesped FK
        int id_domo FK
        date fecha_inicio
        date fecha_fin
        numeric precio_total_estadia
    }
    PAGO {
        int id_pago PK
        int id_reserva FK
        numeric monto_pagado
        string metodo_pago
        string estado_pago
        timestamp fecha_pago
    }
    ACTIVIDAD_AVENTURE {
        int id_actividad PK
        string nombre_actividad
        numeric precio_persona
    }
    REGISTRO_ACTIVIDAD {
        int id_registro PK
        int id_reserva FK
        int id_actividad FK
        int cantidad_personas
        numeric precio_calculado
    }

    TIPO_DOMO ||--o{ DOMO : "id_tipo_domo"
    DOMO ||--o{ RESERVA : "id_domo"
    HUESPED ||--o{ RESERVA : "id_huesped"
    RESERVA ||--o{ PAGO : "id_reserva"
    RESERVA ||--o{ REGISTRO_ACTIVIDAD : "id_reserva"
    ACTIVIDAD_AVENTURE ||--o{ REGISTRO_ACTIVIDAD : "id_actividad"
