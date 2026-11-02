
### Sesión de usuario y Token

Para la persistencia del `JWT`y la sesión, Zustand mantiene los datos del usuario para compartir como un estado centralizado, y su token.

El token a su vez se almacena en el AsyncStorage para su validación aunque no hay renovación automática.

### Cache

Con TanStack organizamos los resultados mediante claves (como los filtros de busqueda), generamos una copia temporal de los datos del backend que se mantienen en memoria. Así evitamos ir a consultar datos que ya habiamos consultado recientemente.

![cache_flow](cache_flow.png)

Al cambiar el token, se cancelan las consultas de actividades y se elimina su caché para evitar reutilizar información privada entre sesiones.

Antes solo lo había utilizado con RTKQuery pero es el mismo principio, se podría agregar una acción para forzar la actualizacion de datos (ademas del cambio de filtros de busqueda) pero eso depende de que tanto cambie la información que se requiere.

### Aplicaciones y Favoritos de Vacantes

Revisamos que acitividades tiene el usuario `applications`y `favorites` para asi comprobar cual vacante marcamos como favorito o a cual ya aplicamos. (y mostrar el componente visual)

Al presionar "postularme ahora" o el icono de "favorito" en el detalle de la vacante, buscamos por el tipo de accion y bifurcamos la acción para mostrar el estado visual mientras se hace la petición al backend.

Es decir "SUPONEMOS" que todo va a salir bien y aplicamos el cambio en el cache de las postulaciones y/o favoritos. Para reflejar el cambio al usuario, mientras se envia la petición al backend.

Al recibir la respuesta del backend ahora si validamos que el usuario, la vacante y el estado sea el mismo y actualizamos el caché.

![apply_activitie](apply_activitie.png)

### Precargado de Vacantes

Dada la posicion de los elementos que estamos que estamos consultando cuando se alcance un umbral de +/- 3 vacantes.

Con TanStack comprobamos si el dato siguiente (en ambas direcciones) lo tenemos en cache o debemos ir a consultarlo para guardar la respuesta o actualizar el dato.

Este proceso lo repetimos con cada movimiento de la vacante en detalle, el caso especial ocurre cuando estamos cerca del final de la lista de datos almacenados y consultemos al endpoint los nuevos datos para anexarlos a la lista.

![cache_preload](cache_preload.png)

Todo este flujo de cache no es recomendable en mi experiencia con las siguientes excepciones:

* Dispositivos objetivo de baja gama, puede generar lentitud y descarga de bateria por la saturación de datos y constantes operaciones de almacenamiento
* Cuando los datos cambian constantemente,  para un escenario realista como una reservación o venta de productos, o un servicio con alta demanda (como inscripciones universitarias):
* Cuando los datos requieren un procesamiento que puede tardar varios segundos (como reportes de datos), para este caso usaria un signal para enviar una notificacion al dispositivo para ahora si consultar los datos cuando esten listo.