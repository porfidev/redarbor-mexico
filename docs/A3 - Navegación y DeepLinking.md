Para la navegación utilizamos la estructura de carpetas recomendada por expo.

Los accesos son publicos a excepcion de las actividades que requieren una sesion, por lo que redirigimos al login si no hay alguna y se valida en Front y Back estas operaciones.

### Deep Linking

Enlaces como `occ://vacante/123`. Validamos el esquema y el destino antes de navegar.

**Si la app está cerrada**, el destino se recupera desde la notificación inicial o el enlace pendiente guardado en AsyncStorage. (por que en este punto antes hay que validar la sesión)

Primero recuperamos los datos guardados y validamos el token con `/auth/me`.

Cuando la sesión está resuelta, procesamos el enlace. La ruta abre el detalle sobre la búsqueda y mostramos la vacante al usuario.

### Rutas y BottomSheetModal

El sheet está montado en el layout raíz. Al tocar una tarjeta, guardamos su ID y los datos de busqueda como para almacenar un estado de lo que queremos mostrar. 

Para el deep link hacemos lo mismo: guardamos el ID y reemplaza los datos de la busqueda.

### Swipe y posición

* Guardamos el ID activo y calculamos su índice dentro de los resultados
* Cada swipe cambia ese ID (al cerrar conservamos como `lastClosedId`)
	* Si la tarjeta sigue visible, dejamos la lista donde estaba (con el scrollToIndex y el indice almacenado)
	* Si la vacante no pertenece a los resultados actuales, no desplazamos la lista.