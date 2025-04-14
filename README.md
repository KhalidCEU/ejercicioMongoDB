# Ejercicio MongoDB

> Estos comandos los ejecutamos dentro del shell de MongoDB **mongosh**


```
show dbs
use sample_training
```

**1)** En `sample_training.zips` ¿Cuántos ~~colecciones~~ documentos tienen
menos de 1000 personas en el campo `pop`?

> Aquí `sample_training` es una DB y `zips` una de sus colecciones.

```
db.zips.countDocuments({ pop: { $lt: 1000 } })
```

**Output**: **8065** colecciones tienen menos de 1000 personas en el campo `pop`;

**2)** En `sample_training.trips` ¿Cuál es la diferencia entre la
gente que nació en 1998 y la que nació después de 1998?

```
const bornIn1998 = db.trips.countDocuments({ 'birth year': 1998 });
const bornAfter1998 = db.trips.countDocuments({ 'birth year': { $gt: 1998 } });

const difference = bornIn1998 - bornAfter1998;
print(`Difference: ${difference}`);
```

**Output**: -6

**3)** En `sample_training.routes` ¿Cuántas rutas tienen al menos
una parada?

```
sample_training> db.routes.countDocuments({ stops: { $gte: 1 } });
```

**Output**: 11

**4)** En `sample_training.inspections` ¿Cuántos negocios
tienen un resultado de inspección "Out of Business" y
pertenecen al sector *"Home Improvement Contractor -
100"?

```
db.inspections.countDocuments({ result: "Out of Business", sector: "Home Improvement Contractor - 100" })
```

**Output**: 4

**5)** En `sample_training.inspections` ¿Cuántos documentos
hay con fecha de inspección "Feb 20 2015" o "Feb 21 2015" y
cuyo sector no sea "Cigarette Retail Dealer - 127"?>>

```
db.inspections.countDocuments({ date: { $in: ["Feb 20 2015", "Feb 21 2015"] }, sector: { $ne: "Cigarette Retail Dealer - 127" } })
```

o usando `$or` para las fechas

```
db.inspections.countDocuments({ $or: [{ date: "Feb 20 2015" }, { date: "Feb 21 2015" }], sector: { $ne: "Cigarette Retail Dealer - 127" } })
```

**Output**: 204


## Otros comandos

`show dbs` : mostrar DB's

`use <db_name>` : acceder a una DB especifica

`show collections`: mostrar colecciones de la DB actual

`db.<collection_name>.find().limit(1)` : consultar 1 solo documento (util para ver su schema)

Número de documentos **total** (suma de todas las colecciones) de la DB :
`db.getCollectionNames().reduce((total, coll) => total + db[coll].countDocuments({}), 0)`

Número de documentos **total** en **cada colección** de la DB :
```
let total = 0;
db.getCollectionNames().forEach(collection => {
  const count = db[collection].countDocuments({});
  total += count;
  print(`${collection}: ${count}`);
});
```

### Notas

Si el field no está en snake_case, por ejemplo `birth year` (separado por 1 whitespace), usar así 'birth year' (entre single quotes al hacer la consulta) (por ej. ejercicio 2).