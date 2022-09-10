**Prueba Corta #2**  
Estudiante: Celina Madrigal Murillo  
Carné: 2020059364

1. Explique el concepto de shard, replica y partition

    **Shard:** es una partición horizontal de datos en una base de datos o motor de búsqueda.

    **Replica:** copia de datos de una base de datos a otra.

    **Partition:** es una división de una base de datos lógica o sus elementos en partes independientes.

2. Explique la diferencia entre Strong Consistency Eventual Consistency

   Strong Consistency nos da datos actualizados pero con alta latencia y Eventual Consistency nos da baja latencia pero puede responder a solicitudes de lectura con datos desactualizados.

3. ¿En que consiste warm replicas y hot replicas?

    **Warm replicas:** Se fuerza consistencia fuerte. No acepta conexiones que piden datos. 

    **Hot replicas:** se trabaja en consistencia eventual. Los nodos aceptan conexiones de lectura.

4. ¿En que consiste consiste switch over y fail over?  
   
    **Switch over:** es una inversión de rol entre la base de datos primaria y una de sus bases de datos en espera.

    **Fail over:** es un evento no planificado que asume que se pierde la base de datos primaria.

**Referencias:**

de, C. (2020, January 15). Shard (arquitectura de base de datos). Wikipedia.org; Wikimedia Foundation, Inc. https://es.wikipedia.org/wiki/Shard_(arquitectura_de_base_de_datos)

de, C. (2009, December 3). división de una base de datos lógica o sus elementos constituyentes en partes independientes. Wikipedia.org; Wikimedia Foundation, Inc. https://es.wikipedia.org/wiki/Partici%C3%B3n_(base_de_datos)

Saurabh.v. (2017, July 16). Eventual vs Strong Consistency in Distributed Databases. Hackernoon.com. https://hackernoon.com/eventual-vs-strong-consistency-in-distributed-databases-282fdad37cf7

‌Planned Switchover. (2021). Oracle Help Center; Despliegue de una topología de DR híbrida para Oracle Exadata local. https://docs.oracle.com/es/solutions/hybrid-dr-for-exadata/planned-switchover1.html#:~:text=Un%20switchover%20es%20una%20inversi%C3%B3n,mantenimiento%20planificado%20del%20sistema%20primario.

Unplanned Failovers. (2021). Oracle Help Center; Despliegue de una topología de DR híbrida para Oracle Exadata local. https://docs.oracle.com/es/solutions/hybrid-dr-for-exadata/unplanned-failovers1.html#:~:text=Un%20failover%20es%20un%20evento,la%20base%20de%20datos%20primaria.

‌
‌

‌

‌