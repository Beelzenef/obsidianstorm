En [[DomainLayer]]

- Asegura que los objetos que encapsula mantienen un estado consistente.
- Cuida de la persistencia
- Se recomienda tener un repositorio (no un repository de EF Core) por Aggregado.
- Genera actualizaciones o eliminaciones en cascada

Implementan la lógica de dominio que no pertenece a ningún agregado en particular, más bien abarca múltiples entidades/modelos

Coordinan la actividad de los [[Aggregates]] y repositorios con el propósito de implementar la acción de negocio.

Puede consumir servicios de infraestructura.