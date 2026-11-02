Aunque estoy algo desactualizado de DEVOPS y las herramientas he utilizado mayormente los errores de:

* Crashalytics
* DynaTrace

en algunos otros proyecto Sentry o SonarQube, así como los linters aplicados y revisores de calidad en el código, nunca falta el evento inesperado que puede ser prevenido con buenas practicas.

### React Native 

El rey serian las DevTools de React Native con su Profiler para revisar el rendimiento.

Otras ayuditas
* Mantener componentes similares para evitar calculos de renders de componentes que varian mucho
* Algoritmos para paginar dependiendo de la calidad de conexion o latencia (variar el tamaño de las peticiones de datos)
* Conservar estados a veces evitar reconstruir componentes pero aumenta el consumo de memoria.

### Nativo

Independientemente de Android o iOS, su IDE da los logs del dispositivo, una serie de filtros para revisar warnin o errores en una compilación debug pero apuntando a produccion, siempre es util.

### Otros

* Analizadores o mapas de calor para ver que funcionalidad realmente esta siendo utilizada y cual no para ser descarta o bajarle la prioridad.
* Si hay muchos procesos iniciales ver cual representan mayor impacto para priorizarlos
* Segmentar la información, un snapshot de un minimo de datos y al ver un detalle ahora si ir por el servicio de datos completo.

### Prefetch

La precarga de vacantes comienza en un efecto cuando hay un detalle activo, entonces hasta empezar a ver un detalle es cuando se aplica la busqueda con el umbral limite.

El cache de los 30 segundos ayuda, y probablemente podamos aumentar el tiempo pues una vacante no cambia mucho, quizá hasta almacenando datos con una fecha de ultima sincronización para no consumir el endpoint hasta después de pasar el tiempo o una acción concreta de actualización forzosa.


### Lo más importante

> "La simplicidad es un requisito previo para la confiabilidad"
Edsger W. Dijkstra