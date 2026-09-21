# Modelo de correspondencias entre puestos, regímenes y artículos

Este documento explica cómo se clasifica cada puesto de `PUESTOS_alineado.xlsx` en un régimen y sus artículos de licencia, y qué reglas se aplicaron. El buscador `buscador_mapa_relacional/index.html` implementa este modelo.

**El Excel de regímenes es la fuente de verdad.** El buscador no es fuente: se corrigió a partir de él.

---

## Cómo se usa el buscador

1. Filtrá por **jurisdicción → organización → cargo**.
2. Mirá la columna **Régimen / candidatos**: si el grupo tiene un solo régimen, aparece el badge `Vinculada`. Si el régimen depende de la revista, aparece `Según revista` con ambos regímenes.
3. Abrí el detalle con **Ver →** para consultar los artículos y la lista de puestos.
4. En la tabla de puestos, la columna **Régimen** indica el régimen de cada puesto según su revista.

---

## Fuente de verdad

| Insumo | Rol |
|---|---|
| `resumen de articulos de licencias (1).xlsx` | Regímenes, alcance por jurisdicción y artículos |
| `cat.17 ss penitenciario cat 21 residentes salud.xlsx` | Catálogos de cargos de Seguridad y de Residencias |
| `PUESTOS_alineado.xlsx` | 133.605 puestos a clasificar |

---

## Reglas de clasificación

Cada regla sale del alcance declarado en el Excel de regímenes.

| Jurisdicción | Condición | Régimen |
|---|---|---|
| 8 | — | Honorable Tribunal de Cuentas |
| 34 | — | Instituto de Cardiología |
| 1 | Cargo 17299–17319 | Servicio Penitenciario |
| 1 | Resto | Administración Pública General (Personal Civil) |
| 4 | Cargo del catálogo de Residencias | Residencias en Salud |
| 4 | Resto | Administración Pública General |
| 3, 15, 23, 28, 33 | Revista Titular o Interino | Personal Docente |
| 3, 15, 23, 28, 33 | Revista Suplente | Personal Docente Suplente |
| 3, 15, 23, 28, 33 | Revista vacía | Administración Pública General (cargo no docente) |
| 37 | Revista Suplente | Personal Docente Suplente |
| 10 | — | **A decidir** |
| Resto de la lista del Excel | — | Administración Pública General |
| Fuera de la lista | — | **Sin correspondencia** |

**La diferenciación por revista es la clave del modelo.** El Excel declara que el régimen suplente alcanza a las jurisdicciones `3-10-15-23-28-33-37`, y que debe diferenciarse por situación de revista. Esa diferenciación es la que el buscador anterior no hacía.

---

## Resultado sobre los 133.605 puestos

| Régimen | Puestos |
|---|---:|
| Personal Docente Suplente | 56.470 |
| Personal Docente | 43.503 |
| Sin correspondencia | 16.315 |
| Administración Pública General | 13.108 |
| Servicio Penitenciario | 2.346 |
| Residencias en Salud | 1.112 |
| Instituto de Cardiología | 592 |
| Honorable Tribunal de Cuentas | 134 |
| A decidir | 25 |

Por estado de la correspondencia:

| Estado | Puestos |
|---|---:|
| Vinculada | 79.421 |
| Según revista | 37.853 |
| Sin correspondencia | 16.315 |
| A decidir | 16 |

---

## Correcciones aplicadas al buscador

| Cambio | Puestos |
|---|---:|
| Suplentes educativos → Docente Suplente (art. 34b y 34G) | 7.697 |
| Jurisdicción 4 → Residencias (catálogo de cargos) | 1.112 |
| Jurisdicción 4 → General (resto) | 7.754 |

**Antes**, los 7.697 suplentes mostraban el art. 8 inc. a, cuyo texto aplica a *titulares e interinos*. **Ahora** muestran el art. 34b, que es el que les corresponde.

---

## Casos sin resolver

Son los únicos apartados abiertos. Están en la hoja `Casos_pendientes` del Excel del modelo y también visibles en el buscador con los filtros `A decidir` y `Sin correspondencia`.

| Tipo | Puestos | Detalle |
|---|---:|---|
| Jurisdicción fuera del Excel | 16.315 | 9 jurisdicciones que no figuran en el Excel de regímenes |
| Régimen a decidir | 25 | 16 en jurisdicción 10 y 9 con revista distinta de Suplente en la 37 |

---

## Archivos

| Archivo | Qué es |
|---|---|
| `buscador_mapa_relacional/index.html` | Buscador con el modelo aplicado |
| `MODELO_CORRESPONDENCIAS_REGIMENES.xlsx` | Tablas del modelo: combinaciones, grupos, artículos y pendientes |
| `index.backup-pre-modelo.html` | Respaldo del buscador anterior, solo local |

---

## Próximo paso

Resolver los 25 puestos con régimen a decidir y definir las 9 jurisdicciones sin correspondencia. Con eso cerrado, el modelo queda completo.
