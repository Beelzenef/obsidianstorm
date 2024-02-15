Hay que diferenciar el modelo de dominio y el modelo de persistencia.

No es buena idea añadir [[Behaviour]] a un modelo de persistencia, es muy posible que:

- Genere conflicto con el [[UbiquitousLanguage]]
- Genere conflicto a la hora de expresar reglas de negocio

La gestión de base de datos es:

- infraestructura
- una limitación

El modelo que se perfila usando EF Core y code-first tiende más a [[AnemicModel]]