# 📝 Planteamiento del Problema - Sistema de Gestión de Glamping

## 1. Contexto del Negocio
El turismo de naturaleza y el hospedaje de lujo en formatos como el *Glamping* han experimentado un crecimiento exponencial en los últimos años. Este modelo de negocio combina la experiencia de acampar al aire libre con las comodidades de un hotel de alta gama, ofreciendo domos geodésicos equipados, servicios VIP (como jacuzzis privados) y paquetes de actividades de aventura ecoturística (senderismo, canopy, cabalgatas).

## 2. Descripción de la Problemática
A pesar de su alta demanda y tarifas premium, el Glamping objeto de este estudio presenta graves ineficiencias operacionales debido a la ausencia de un sistema de información centralizado. Actualmente, la gestión se realiza de forma manual mediante hojas de cálculo distribuidas (Excel) y registros en papel, lo que genera los siguientes problemas críticos:

* **Descontrol en la Disponibilidad de Domos:** No existe una sincronización en tiempo real cuando se genera una reserva. Esto causa riesgo de sobreventa (*overbooking*) o domos desocupados que figuran como reservados por error.
* **Falta de Integración con Servicios Adicionales:** Las actividades de aventura se registran de forma aislada a las estadías de los huéspedes, lo que genera errores humanos al momento de liquidar la cuenta final y pérdida de ingresos por servicios no cobrados.
* **Inconsistencia en los Recaudos de Caja:** Al no asociarse estrictamente cada pago a una reserva específica y un medio de pago parametrizado, el balance financiero presenta descuadres frecuentes entre el dinero reportado y la ocupación real.

## 3. Justificación de la Solución Relacional
Para mitigar estas fallas, se diseñó e implementó una base de datos relacional robusta bajo el motor PostgreSQL, automatizada mediante el uso de herramientas de migración como Liquibase y contenedores Docker. 

La base de datos centraliza los datos de los huéspedes, el inventario de domos por categorías, el catálogo de actividades de aventura y los estados financieros de las transacciones. De este modo, la persistencia de datos garantiza la consistencia del negocio mediante restricciones de integridad, funciones de cálculo de subtotales y disparadores (*triggers*) operacionales automáticos.
