# Modelo de correspondencias entre puestos, regímenes y artículos

Este documento explica cómo se clasifica cada puesto de `PUESTOS_alineado.xlsx` en un régimen y sus artículos de licencia, y qué reglas se aplicaron. El buscador `buscador_mapa_relacional/index.html` implementa este modelo.

**El Excel de regímenes es la fuente de verdad.** El buscador no es fuente: se corrigió a partir de él.

---

## Cómo se usa el buscador

1. Filtrá por **jurisdicción → organización → cargo**.
2. Mirá la columna **Régimen / candidatos**: si el grupo tiene un solo régimen, aparece el badge `Vinculada`. Si el régimen depende de la revista, aparece `Según revista` con ambos regímenes.
3. Abrí el detalle con **Ver →** para consultar los artículos y la lista de puestos.
4. En la tabla de puestos, la columna **Régimen** indica el régimen de cada puesto.

---

## Fuente de verdad

| Insumo | Rol |
|---|---|
| `resumen de articulos de licencias (1).xlsx` | Regímenes, alcance por jurisdicción y artículos |
| `cat.17 ss penitenciario cat 21 residentes salud.xlsx` | Catálogos de cargos de Seguridad y de Residencias |
| `PUESTOS_alineado.xlsx` | 133.605 puestos a clasificar |

---

## Cómo se decide el régimen

El régimen no depende de una sola clave. Se decide por **agrupamiento del cargo** y, dentro de lo docente, por **situación de revista**.

**Agrupamiento del cargo (campo `categoria`):**

| Agrupamiento | Categorías | Docente |
|---|---|---|
| Docente (escalafón) | 5 | Sí |
| Horas cátedra | 7 | Sí |
| Autoridades | 1 | No |
| Personal administrativo y técnico | 2 | No |
| Obrero y maestranza | 10 | No |
| Servicio | 11 | No |
| Personal técnico / informática | 12 | No |
| Profesional universitario | 9 | A decidir |
| Directores | 19 | A decidir |
| Beca alfabetización | 41 | A decidir |
| Becas | 80 | A decidir |

---

## Reglas de clasificación

| Jurisdicción | Condición | Régimen |
|---|---|---|
| 8 | — | Honorable Tribunal de Cuentas |
| 34 | — | Instituto de Cardiología |
| 1 | Cargo del catálogo penitenciario | Servicio Penitenciario |
| 1 | Resto | Administración Pública General (Personal Civil) |
| 4 | Cargo del catálogo de Residencias | Residencias en Salud |
| 4 | Resto | Administración Pública General |
| 3, 15, 23, 28, 33, 37, 10 | Cargo no docente (categorías 1, 2, 10, 11, 12) | Administración Pública General (Ley 4.067) |
| 3, 15, 23, 28, 33, 37, 10 | Cargo docente (categorías 5, 7) con revista Titular o Interino | Personal Docente |
| 3, 15, 23, 28, 33, 37, 10 | Cargo docente (categorías 5, 7) con revista Suplente | Personal Docente Suplente |
| 3, 15, 23, 28, 33, 37, 10 | Cargo docente sin situación de revista | **A decidir** |
| 3, 15, 23, 28, 33, 37, 10 | Categoría sin agrupamiento definido | **A decidir** |
| Resto de la lista del Excel | — | Administración Pública General |
| Fuera de la lista | — | **Sin correspondencia** |

**Dos claves, no una.** El agrupamiento del cargo decide *si* el puesto es docente; la revista decide *cuál* régimen docente. Un cargo no docente nunca entra al régimen docente, aunque su revista sea Titular. El buscador anterior clasificaba solo por revista, y por eso asignaba cargos administrativos, de servicio y de maestranza al régimen docente.

---

## Resultado sobre los 133.605 puestos

| Régimen | Puestos |
|---|---:|
| Personal Docente Suplente | 56.456 |
| Personal Docente | 41.508 |
| Sin correspondencia | 16.315 |
| Administración Pública General | 15.110 |
| Servicio Penitenciario | 2.346 |
| Residencias en Salud | 1.112 |
| Instituto de Cardiología | 592 |
| Honorable Tribunal de Cuentas | 134 |
| A decidir | 32 |

Por estado de la correspondencia:

| Estado | Puestos |
|---|---:|
| Vinculada | 79.801 |
| Según revista | 37.466 |
| Sin correspondencia | 16.315 |
| A decidir | 23 |

---

## Cambios sobre el modelo anterior

| Régimen | Antes | Ahora |
|---|---:|---:|
| Personal Docente | 43.503 | 41.508 |
| Personal Docente Suplente | 56.470 | 56.456 |
| Administración Pública General | 13.108 | 15.110 |
| A decidir | 25 | 32 |

- **Cargos no docentes de jurisdicciones educativas → General.** Los cargos de categorías 1, 2, 10, 11 y 12 de las jurisdicciones 3, 15, 23, 28, 33 y 37 dejaron el régimen docente y pasaron a Administración Pública General (Ley 4.067).
- **Jurisdicción 10 → General.** Sus 16 puestos son cargos no docentes (autoridades, administración y técnicos), así que se resolvieron a General.
- **Suplentes educativos → Docente Suplente.** Se mantiene la corrección anterior: la revista Suplente usa art. 34b y 34G.

---

## Casos sin resolver

Están en la hoja `Casos_pendientes` del Excel del modelo y visibles en el buscador con los filtros `A decidir` y `Sin correspondencia`.

| Tipo | Puestos | Detalle |
|---|---:|---|
| Jurisdicción fuera del Excel | 16.315 | 9 jurisdicciones que no figuran en el Excel de regímenes |
| Categoría sin agrupamiento | 23 | Categorías 9, 19, 41 y 80 de jurisdicciones educativas |
| Cargo docente sin revista | 9 | Cargos docentes de jurisdicción 37 con revista vacía |

---

## Archivos

| Archivo | Qué es |
|---|---|
| `buscador_mapa_relacional/index.html` | Buscador con el modelo aplicado |
| `MODELO_CORRESPONDENCIAS_REGIMENES.xlsx` | Tablas del modelo: combinaciones, grupos, artículos y pendientes |
| `index.backup-pre-modelo.html` | Respaldo del buscador anterior, solo local |

---

## Próximo paso

Definir el agrupamiento de las categorías 9, 19, 41 y 80, y la revista de los cargos docentes de la jurisdicción 37. Con eso cerrado, el modelo queda completo.
