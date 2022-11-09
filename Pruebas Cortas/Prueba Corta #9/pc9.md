**Prueba Corta #9**  
Estudiante: Celina Madrigal Murillo  
Carné: 2020059364  

1. Suponiendo que un sistema de bases de datos relacional que presenta un read-heavy workload y los queries son muy diferentes, explique detalladamente ¿porque el uso de caches puede afectar el rendimiento del sistema de forma negativa? (30 pts)

    Normalmente el almacenamiento en caché es más eficaz cuando una instancia de cliente lee de forma repetida los mismos datos pero al presentar un read-heavy workload y que los queries sean muy diferentes no son las mismas solicitudes y además son pesadas entonces al ser diferentes todas se guardan en cache, este se hace muy grande y por eso baja el rendimiento.

2. El particionamiento de tablas en bases de datos relacionales es un concepto muy parecido al de shards en bases de datos NoSQL, explique detalladamente ¿Cómo afecta el particionamiento y el sharding en el rendimiento de bases de datos SQL y NoSQL? (30 pts)

    El particionamiento o sharding de bases de datos es un tipo de particionamiento horizontal que divide las bases de datos de gran tamaño en componentes más pequeños, que son más rápidos y fáciles de administrar por ende nos ofrece accesos de gran velocidad. No importa si disponemos de servidores en distintas zonas del mundo, el sharding se ocupa de que se reduzca el volumen de latencia y el rendimiento sea superior.

3. En un sistema de bases de datos con Strong Consistency cuyo workload es de read-heavy y write-heavy, ¿Cómo afectan los exclusive locks el rendimiento de las bases de datos NoSQL? (20 pts)

    Los bloqueos pueden ser muy costosos en cuanto a recursos y, por tanto, sólo se deben utilizar cuando sea necesario para preservar la integridad de los datos. En una base de datos en la que cientos o miles de usuarios podrían estar intentando obtener acceso a un registro cada segundo, el uso de bloqueos innecesarios podría reducir el rendimiento de la aplicación.

4. Explique detalladamente, ¿Cómo afecta la selección de discos físicos el rendimiento de una base de datos SQL y NoSQL? (20 pts)

    Los discos SSD son mucho más rápidos que los HDD por ende si se escoge uno HDD el rendimiento será mucho menor a diferencia si se escoge uno SDD.


**Referencias**:

‌v-aangie. (2022, September 22). Almacenamiento de datos en caché para optimizar el rendimiento - Microsoft Azure Well-Architected Framework. Microsoft.com. https://learn.microsoft.com/es-es/azure/architecture/framework/scalability/optimize-cache

‌Sharding, ventajas y desventajas de su aplicación - ADN Cloud. (2019, August 20). #ADNCLOUD. https://blog.mdcloud.es/sharding-ventajas-y-desventajas/

‌¿Qué es el particionamiento de bases de datos? | Microsoft Azure. (2022). Microsoft.com. https://azure.microsoft.com/es-es/resources/cloud-computing-dictionary/what-is-database-sharding

o365devx. (2021, September 25). ¿Qué es un bloqueo? (Referencia de base de datos de escritorio de Access). Microsoft.com. https://learn.microsoft.com/es-es/office/client-developer/access/desktop-database-reference/what-is-a-lock

‌
‌

‌





