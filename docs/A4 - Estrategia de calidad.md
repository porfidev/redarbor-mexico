Por el momento no he incluido pruebas unitarias, pero usaria Jest solo para el comportamiento base de los componentes.

Al ser componentes aislados es facil comprobar su funcionamiento von mooks, asigando una IA para generar las pruebas y revisar si los casos cumplen con la funcionalidad.

### Commits

Utiliaria conventional commits, no lo hice muy estricto en este caso pero se puede implementar un linter o una tarea en el pre-commit para revisar que los mensajes sean coherentes con las reglas.

Adicionalmente quiza tener una referencia a una tarea en especifico en el sistema de gestión o validar pruebas.

Por preferencia prefiero revisar antes de hacer Push, en vez de cada commit pues ocurre que aveces necesitamos cambiarnos de tarea y tener un stash de funcionalidades sin terminar suele ser más dificil de administrar que una rama con funcionalidades en desarrollo.

### Git 

Para el control de versiones algo basado en gitFlow o Troncal depende mucho de la experiencia del equipo y de la distribución del trabajo, mayormente feat, fix con sus ramas de release y qa cumplen el proposito.

La preferencia es en la integración de commits por el REBASE, a diferencia de una mezcla de Merge, con Rebase el desarrollador que lleva su feature en su rama es el que tiene la responsabilidad de integrar y resolver confilctos antes de hacer cualquier integración a DEV.

Además la estrategia de Rebase con SQUASH para que cada feature tenga una integración con un unico commit, en caso de que se ocupe eliminar o revisar, un commit y una rama en especifico es mas facil integrarlo que una rama con varios commits. La desventaja es cuando se quiere una feature "a medias" pero todo se puede planear.


### Pull Request

Yo creo que al cubrir con los linters, prettier y hooks de commits con las reglas de estructura todo está mayormente cubierto.

El PR ya sería el tramite para validar con el equipo si hay aluna afectación o comentario de mejora.

A partir de aquí depende del sistema de Gestion, Jira, Azure, Click etc... a veces tienen integraciones que relacionan el ticket con los commits y es más facil relacionarlos para darle seguimiento al trabajo.

Personalmente la estructura del nombre de rama:

`feat/<Ticket o Tarea>-<actividad desarrollada>` cumple bastante y al hacer un squash, cada uno de esos commits quedan en un resumen paso a paso de lo que se hizo.