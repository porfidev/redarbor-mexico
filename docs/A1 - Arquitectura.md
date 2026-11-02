## Backend

Por la naturaleza del proyecto que es una prueba prototipo y los requerimientos de almacenamiento en memoria opté por el patrón **Controlador -> Servicio -> Repositorio**

Estoy ya que es una forma familiar de trabajar con Spring Boot el cual estoy retomando para una pruebas técnicas así que aproveche para practicarlo.

![pattern](pattern.png)


### Separación por módulos

Cada funcionalidad tiene su propia serie de capas dentro de cada modulo, para facilitar cuando un modulo deba ser reemplazado, eliminado o incluso deba reutilizarse en otro proyecto, tan fácil como llevarse la carpeta y agregar al orquestador principal (el indice de rutas)

### Incrementar funcionalidades

Si se quiere escalar el proyecto se prueden aplicar cambios parciales sin mucha afectación al resto de modulos.

* Almacenamiento: Cambiar de datos en memoria a una conexión a base de datos, o incluso servicios externos
* Nuevas funcionalidades: Solo agregar un modulo con la estructura de archivos
* Optimización: se pueden mejorar los algoritmos o consultas en partes concretas del servicio

### Trabajo en equipo

Al estar los modulos separados, el desarrollo se puede hacer con menos dependencia o interferencia incluso si no se trabaja en el mismo equipo.

Se pueden hacer pruebas a QA/Producción con modulos desarrollados, pero desactivados o no incorporados sin afectar la funcionalidad.

Se pueden desactivar módulos experimentales o que su salida no esta planeada, aun cuando el código ya se encuentre en el repositorio.

---

### Funcionalidad de carpetas en BE

| Carpeta                 | Resumen funcionalidad |
| ----------------------- | ------------------------------------------------ |
| `data/seeds:`           | Datos iniciales utilizados por los repositorios. |
| `modules/auth/`         | Autenticación y administración de sesiones.      |
| `modules/jobs/`         | Consulta, búsqueda y detalle de vacantes.        |
| `modules/applications/` | Gestión de postulaciones a vacantes.             |
| `modules/favorites/`    | Gestión de vacantes favoritas.                   |
| `modules/common/`       | Código compartido entre módulos.                 |

### Módulo Común

El critero para colocar algo aquí es que se utilice por más de un módulo o funcionalidad, como funciones utilitarias, tipados, middlewares y manejor de errores

| Carpeta Commons | Resumen                                      |
| --------------- | -------------------------------------------- |
| `middleware/`   | Procesamiento compartido de peticiones HTTP. |
| `messages/`     | Mensajes compartidos del backend.            |
| `utils/`        | Funciones auxiliares reutilizables.          |
| `types/`        | Tipos compartidos del backend.               |

### Common Messages

La idea es la traducción o regionalización, no termine de ajustarlo pero dependiendo la región se pueden enviar mensajes personalizados por idioma, además de tener centralizado el conjunto de mensajes que después se puede sustituir por una libreria especializada.

Solo puse por ahora los códigos de error básicos del consumo de la API.

### Framework Hono

Bueno en realidad tiene poco tiempo que lo descubrí habia trabajado con Hapi y Next pero quize aprovechar la ocasión para un proyecto más complejo.
#### Cosas que me gustan
* Datos de sesión tipados en la aplicación `AppEnv`
* Estándares web comunes como Request, Response y Headers
* Tiene dependencias internas,  como las de JWT y CORS

No representa una ventaja muy significativa vs express pero es una buena alternativa.

### ESLINT y Prettier

Aunque hoy en día la I.A. sustituye muchas actividades repetitivas, la calidad del código debe seguir siendo un estándar y una frase que me gusta

> Escribir código que entienda la máquina es sencillo....
> escribir código que entienda otra persona, eso es lo dificil.

Añadi estás librerias para tratar de mantener un estandar visual y funcional que sea amigable con cualquier persona que tome el proyecto.
## FrontEnd

Similar a backend pero para frontend se partió de una **arquitectura por funcionalidades (_feature-based_)**, con **componentes, hooks y servicios** para separar responsabilidades.

Y aunque parece un MVVM cuando hablamos de frontend con react algunas componentes requieren una logica interna para su funcionamiento, mientras que un grupo de componentes "orquestadores" requieren otra lógica.

### Estructura de carpetas FrontEnd

El criterio para decidir si algo es modular o compartido es sencillo, si solo lo ocupa la `feature` entonces entra en una una subcarpeta del propio feature o funcionalidad, en el caso de que lo ocupe más de un `feature` como por ejemplo los componentes de `UI` en ese caso pasa a una carpeta superior para consumirse.  

| Carpeta       | Funcionalidad                                                      |
| ------------- | ------------------------------------------------------------------ |
| `app/`        | Define rutas, pantallas de entrada y navegación de la aplicación.  |
| `components/` | Contiene componentes visuales reutilizables entre funcionalidades. |
| `config/`     | Configura y valida variables de entorno.                           |
| `constants/`  | Centraliza constantes, colores y valores del tema visual.          |
| `features/`   | Agrupa funcionalidades por modulos.                                |
| `hooks/`      | Contiene hooks reutilizables entre modulos.                        |
| `messages/`   | Centraliza los mensajes mostrados al usuario.                      |
| `providers/`  | Configura proveedores globales, consultas y caché de datos.        |
| `schemas/`    | Define esquemas para validar datos de la API.                      |
| `services/`   | Centraliza la comunicación HTTP con el backend.                    |
| `types/`      | Define tipos y códigos compartidos entre funcionalidades.          |
| `utils/`      | Proporciona funciones auxiliares.                                  |

Formato de Componentes

Para mantener componentes cortos y legibles, separar su aspecto visual reduce el tamaño del archivo y nos permite manipular solo los aspectos visuales en su archivo correspondiente.

Dejando solo la funcionalidad y maquetado en el archivo principal

* `Componente.tsx`
* `Componente.styles.ts` 

Features

Cada feature separa un modulo de funcionalidad de la aplicación, lo que nos permite agregar o quitar sin afectar mucho a las otras funcionalidades.

Solo llega a impactar en el caso de que se tenga una dependencia de un archivo compartido, en ese caso hay que validar que funcionalidad compartida debe alterarse o mantenerse.

| Elemento                 | Funcionalidad                                                                         |
| ------------------------ | ------------------------------------------------------------------------------------- |
| `components/`            | Componentes visuales específicos de la funcionalidad (Mayormente componentes tontos). |
| `screens/`               | Pantallas que integran componentes y comportamiento. (Orquestadores)                  |
| `hooks/`                 | Hooks que coordinan consultas, acciones y estado.                                     |
| `<feature>.service.ts`   | Operaciones para comunicarse con el backend.                                          |
| `<feature>.schema.ts`    | Esquemas para validar entradas y respuestas.                                          |
| `<feature>.types.ts`     | Tipos exclusivos de la funcionalidad.                                                 |
| `<feature>.store.ts`     | Estado compartido del cliente dentro de la funcionalidad.                             |
| `<feature>.keys.ts`      | Claves para identificar consultas y administrar su caché.                             |
| `<Componente>.styles.ts` | Estilos del componente, junto a su archivo.                                           |
| `utils/`                 | Funciones auxiliares exclusivas de la funcionalidad.                                  |
| `constants/`             | Valores fijos y opciones propios de la funcionalidad.                                 |
