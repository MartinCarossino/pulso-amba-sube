# Bitácora de verificación

Cada afirmación del proyecto con su evidencia: qué se controló, cómo y con qué resultado. Las secciones del notebook se citan como "sección NN".

---

## 1. El dataset crudo

**Fuente:** Secretaría de Transporte, *SUBE - Cantidad de transacciones (usos) por fecha* (CC BY 4.0). Un CSV por año (2024, 2025 y 2026), con una fila por día, línea y empresa.

| Control | Resultado |
|---|---|
| Descarga de referencia | 16/10/2026: **1.517.680 filas**, del 01/01/2024 al 14/10/2026 (1.018 días). Sumas anuales: 2024, 4.135.387.458; 2025, 3.910.145.584 |
| Lectura de los archivos | UTF-8, separador coma, sin errores |
| Columnas del diccionario oficial | Las 8 presentes (`dia_transporte`, `nombre_empresa`, `linea`, `amba`, `jurisdiccion`, `provincia`, `municipio`, `cantidad`), más 2 no documentadas: `tipo_transporte` y `dato_preliminar` |
| Fechas o cantidades inválidas | 0 |
| Filas duplicadas (idénticas o por clave natural) | 0 |
| Días sin ningún registro | 0 |
| Cantidades negativas | 4 filas, todas de la línea Mitre (SOFSE): −3 (20/02/2024), −105 (05/03/2024) y −1 (20/04/2024) del ramal Capilla, y −1 (03/05/2025) del ramal Tigre. Total: −110. Son reversas netas: en los análisis se tratan como cero y en la tabla de hechos se conservan tal cual |
| Cantidades en cero | 20 filas (puede ser una línea sin servicio ese día) |
| Cobertura de líneas | 13 días con menos del 80% de la mediana de líneas por día (1.368, total país): 6 feriados, 3 paros y 4 días con faltantes de líneas **solo en el interior** (ver abajo) |
| Datos preliminares | Hasta el corte del 31/08/2026 pesan el 0,010% de las transacciones. Los días posteriores al corte son preliminares y quedan fuera del análisis |
| Provincia `JN` | Es una categoría (jurisdicción nacional), no un relleno: 144.392 filas en la descarga de referencia, todas de jurisdicción NACIONAL |
| Actualización de la fuente | El portal informa "última actualización: 8 de abril de 2026", pero el CSV se actualiza a diario. Entre descargas sucesivas se agregaron días nuevos y septiembre de 2026 pasó a consolidado; los datos ya consolidados no cambiaron |

**Criterio de los controles:** los valores que crecen con la fuente (preliminares, cantidad de días) se calculan en el momento; los que no deberían cambiar (meses consolidados) se fijan a mano, para que una modificación del pasado haga fallar el control.

**Cobertura de líneas con referencia local.** La cantidad de líneas creció de unas 1.380 por día en 2024 a unas 1.640 en 2026, así que un umbral fijo contra la mediana de todo el período marca días de principios de 2024 sin que falten datos. Comparados con el mismo día de la semana, ±14 días, separando AMBA e interior:

| Día | Líneas AMBA / vecinos | Líneas interior / vecinos | Transacciones interior vs. vecinos |
|---|---|---|---|
| Dom 14/01/2024 | 399 / 399 | 703 / 746 | 93,2% |
| Dom 18/02/2024 | 402 / 400 | 693 / 741,5 | 98,2% |
| Mar 05/03/2024 | 413 / 410,5 | 624 / 963 | 60,5% |
| Dom 10/03/2024 | 399 / 400,5 | 691 / 741,5 | 96,1% |

En los cuatro días el AMBA estuvo completo: no son días atípicos para este análisis. El control de la sección 02 mantiene el umbral global como alerta, y la sección 03 explica estos casos.

**Corte del período consolidado: 31/08/2026.** Septiembre ya figura consolidado en la fuente, pero el corte se mantiene para esta publicación: un mes recién consolidado todavía puede tener ajustes.

### Verificación del modelo (sección 05)
**17 de 17 controles en OK.** Las 1.440.295 filas hasta el corte coinciden con las de la fuente; la vista de análisis tiene 1.440.284 (sin los 11 duplicados de la línea 99) y hay 19 eventos verificados.

---

## 2. Contraste con el Banco Central (fuente independiente)

**Fuente:** BCRA, *Informes Mensuales de Pagos Minoristas* (viajes con tarjeta SUBE y Viaje con QR, todo el país), cargados a mano en `datos_bcra.csv` con el informe de origen de cada cifra.

**Convención de meses**, confirmada en la nota al pie de cada informe: el informe del mes M trae la tarjeta SUBE del mes M−1 y el Viaje con QR del mes M. El CSV guarda cada cifra en el mes de los datos.

| Cifra (mes de los datos) | Valor | Informe |
|---|---|---|
| Tarjeta 2025-03 | 348 M (+6,3% i.a.) | Abril 2025 |
| Tarjeta 2025-04 | 363,5 M (−9,0% i.a.) | Mayo 2025 |
| Tarjeta 2026-01 / 02 / 03 / 04 | 240,4 / 222,7 / 311,5 / 305,6 M | Febrero, marzo, abril y mayo 2026 |
| QR 2026-01 / 02 / 03 / 04 | 15,9 / 16,5 / 24,0 / 26,1 M | Enero, febrero, marzo y abril 2026 |
| Tarjeta 2025-07 y 2026-07 | 349,7 M y 270,2 M (−22% i.a.) | Agosto 2025 y agosto 2026 |
| Inicio del Viaje con QR | 12/05/2025 | Prensa; en enero–abril de 2025 el QR del BCRA es cero |

**Coherencia interna (sección 10.1).** Cada informe publica la tarjeta y su variación interanual; con las dos se reconstruye el mismo mes del año anterior y se compara con lo cargado de otro informe. 5 de 6 pares coinciden dentro del redondeo (±0,2%). Julio de 2025 queda en REVISAR: reconstruido 346,4 M contra 349,7 M cargado (−0,94%). Las dos cifras están bien transcriptas; la diferencia es interna del BCRA y no afecta ninguna cifra publicada.

**Distancia entre fuentes (sección 10.2).** Hasta octubre de 2025, el dataset (total país) quedó entre un **2% y un 8% por debajo** de los viajes con tarjeta SUBE que informa el BCRA. La distancia varía mes a mes:

| Mes | Diferencia dataset vs. BCRA |
|---|---|
| Dic 2024 | −7,8% |
| Ene 2025 | −8,1% |
| Feb 2025 | −4,4% |
| Mar 2025 | −5,5% |
| Abr 2025 | −4,5% |
| Jul 2025 | −5,0% |
| Ago 2025 | −2,1% |
| Sep 2025 | −8,4% |
| Oct 2025 | −5,3% |
| Dic 2025 | −0,7% |
| 2026 (ene–jul) | entre +2% y +7% |

El AMBA queda alrededor de un 25% debajo del BCRA, lo que confirma que el BCRA informa todo el país. El AMBA concentra alrededor del 79% de las transacciones.

**Pregunta abierta: ¿el dataset incluye los pagos con QR?** Desde diciembre de 2025 el dataset supera a la tarjeta SUBE del BCRA. Si incluyera el QR, la relación dataset / (tarjeta + QR) debería volver al rango sin QR (0,919–0,956). En 2026 da 0,947–0,985: parte adentro y parte por encima; en octubre de 2025, 0,914. La evidencia es mixta: **plausible, no demostrado.** No se afirma en la publicación.

**Alcance de "tarjeta + QR".** Los validadores del sistema SUBE también aceptan tarjetas bancarias sin contacto ([fuente oficial](https://www.argentina.gob.ar/noticias/amba-mas-de-50-lineas-de-colectivos-incorporan-nuevos-medios-de-pago)), que no aparecen en ninguna de las dos series del BCRA. Por eso la caída de "tarjeta + QR" (−9,8%) es la de los dos medios que mide el BCRA, no necesariamente la caída total.

---

## 3. Días atípicos (19 eventos verificados)

Un evento verificado es un día atípico **para el AMBA**, con su fuente. Esos días se excluyen del "día hábil típico".

### Paros confirmados en la prensa

| Fecha | Qué pasó | Fuente |
|---|---|---|
| 24/01/2024 | Paro general CGT; transporte urbano hasta las 19 h | https://www.expreso.ec/actualidad/mundo/argentina-alista-huelga-general-jueves-199268.html |
| 11/04/2024 | Paro de colectivos UTA: unas 140 líneas sin servicio; DOTA funcionó | https://www.tiempoar.com.ar/ta_article/paro-de-colectivos-cuales-son-las-lineas-que-no-prestan-servicio-y-hasta-cuando/ |
| 09/05/2024 | Paro general CGT: colectivos, trenes y subte (42 líneas del AMBA funcionaron) | https://www.lanacion.com.ar/politica/cuando-es-el-paro-general-de-la-cgt-y-el-transporte-nid07052024/ |
| 30/10/2024 | Paro de trenes y subte; la UTA levantó su paro (fuente débil: página de titulares, coincide con la firma de los datos) | https://www.diagonales.com/paro_t6241bf027e014e0475e231cd |
| 10/04/2025 | Paro general CGT: trenes y subte; la UTA acató la conciliación y hubo colectivos | https://eleconomista.com.ar/politica/todo-lo-tenes-saber-sobre-paro-general-10-abril-n83742 |
| 06/05/2025 | Paro de colectivos UTA (24 h); DOTA funcionó | https://www.canal26.com/general/2025/05/06/paro-de-colectivos-del-martes-6-de-mayo-que-lineas-se-adhieren-a-la-medida-de-fuerza-y-cuales-funcionan/ |
| 19/02/2026 | Paro general CGT contra la reforma laboral | https://www.bloomberglinea.com/latinoamerica/argentina/a-que-hora-se-levanta-el-paro-de-la-cgt-y-como-impacta-en-colectivos-trenes-el-subte-y-vuelos/ |

### Impacto de los paros
Método (`vista_dia_vs_habil_tipico`, sección 04): **solo AMBA**, cada día contra la mediana de los días hábiles del mismo día de la semana, ±35 días. Medido sobre el total país, el interior "amortigua" un paro del AMBA; contra todo el período, se pierde la tendencia.

| Fecha | Total | Colectivo | Tren | Subte |
|---|---|---|---|---|
| 09/05/2024 · paro general | 4,8% | 5,8% | 0,0% | 0,0% |
| 19/02/2026 · paro general | 8,0% | 9,5% | 0,0% | 0,0% |
| 11/04/2024 · paro de colectivos | 33,0% | 20,5% | 80,2% | 101,1% |
| 06/05/2025 · paro de colectivos | 27,3% | 18,1% | 67,4% | 100,4% |
| 10/04/2025 · paro general con colectivos | 53,1% | 62,5% | 0,0% | 0,0% |

### Otros eventos
- **Feriados** inamovibles (1/1, 1/5, 25/12). El calendario completo viene de ArgentinaDatos.
- **Vísperas:** 24 y 31 de diciembre de 2024 y 2025 (caen alrededor del 50%; no son feriados).
- **Jueves Santo:** 28/03/2024 y 17/04/2025 (día no laborable).

### Verificación del calendario de feriados (sección 07)
Los **44** feriados que cayeron en día hábil hasta el 31/08/2026 muestran caída, sin excepciones: entre el 13,8% (Navidad 2025) y el 62,6% (puente del 21/11/2025). Ninguno quedó cerca del 100%, que habría delatado un calendario incorrecto. Los puentes de 2024 eran feriados (32–46%); desde 2025 son días no laborables (46–63%), con más movimiento.

### Días con caída sin causa confirmada
**22 días hábiles** sin evento caen por debajo del 80% de su referencia local (sección 07). Quedan como días hábiles, sin inventarles una causa; las medianas los toleran.
- 15 con caída pareja o casi pareja, concentrados en julio y agosto (vacaciones de invierno) y algunos días sueltos.
- 7 con el colectivo bastante más abajo que el subte (12/03 y 20/03/2024; 05/02, 16/05 y 19/08/2025; 15/07 y 06/08/2026). Ninguno se confirmó en la prensa.
- 6 de los 22 son de julio de 2026 (contra 2 de julio de 2025).

### Prueba de sensibilidad: ¿las cifras publicadas dependen de esos días?
Se recalculan 25 cifras de los posts 1 y 2 en tres escenarios: (A) como se publicó; (B) sin los 7 días en que el colectivo cae mucho más que el subte; (C) sin los 22 días. El escenario A reproduce las 25 cifras publicadas.

| Resultado | B · sin los 7 días | C · sin los 22 días |
|---|---|---|
| Cifras que se mantienen | 20 de 25 | 17 de 25 |
| Post 1 (18 cifras) | Las 18 se mantienen | 16 de 18: día hábil 10,59 M (publicado 10,5) y puente máximo 62,1% (publicado 63%) |
| Post 2, variación 2026 | Se mueve 0,1–0,2 puntos: total −11,9; colectivo −12,1; tren −13,6; subte −10,6; panel −11,0 | Se mueve hasta 0,9 puntos: total −11,1; julio −14,6 |

Lectura:
- **El post 1 no depende de esos días** en el escenario B. C saca también lluvias y vacaciones de invierno, que son viajes reales: es un límite, no un escenario válido.
- **El post 2 no cambia de mensaje.** En B la caída sale apenas mayor (−11,9 en vez de −11,8): 5 de los 7 días son de 2024 y 2025, así que sacarlos sube más la base de 2025 que el nivel de 2026. El "12%" se mantiene.
- **Julio, −20%,** se mantiene en B (−19,9). En C baja a −14,6 porque 6 de los 22 días son de julio de 2026; las vacaciones están en los dos años comparados, así que dejar esos días es lo correcto.

---

## 4. Traspaso de 31 líneas de Nación a Ciudad

- **Acta:** 03/09/2024 (https://eleconomista.com.ar/politica/milei-firmo-jorge-macri-traspaso-31-lineas-colectivos-n77183/amp).
- **Lista oficial:** 4, 6, 7, 12, 23, 25, 26, 34, 39, 42, 44, 47, 50, 61, 62, 64, 65, 68, 76, 84, 90, 99, 102, 106, 107, 108, 109, 115, 118, 132, 151.
- **En los datos:** las 31 líneas con jurisdicción C.A.B.A. coinciden exactamente con el acta (sección 06.2).
- **Corte en el sistema: 01/02/2025**, casi 5 meses después de la firma. La línea 102 aparece como C.A.B.A. desde el 20/01/2025.
- **Días superpuestos:** en los días de transición, la suma de las dos jurisdicciones ronda el 100% de un día normal: es un reparto, no una duplicación (sección 06.3).
- **Regla del modelo:** `numero_linea` une la serie de cada línea antes y después del traspaso, y una línea con código `CABA_LINEA_` pertenece a C.A.B.A., declare lo que declare.

El control de duplicados del dataset crudo no podía detectar este caso porque la jurisdicción es parte de la clave: un control vale lo que vale la clave que usa.

---

## 5. Doble conteo de la línea 99 y fusiones de líneas

- Del 01/02 al 12/02/2026, la línea 99 aparece como NACIONAL (con código `CABA_LINEA_99`) y como C.A.B.A. con cantidades casi idénticas: la suma supera el 200% de un día normal en 10 días, con un máximo de 281,6% (sección 06.3, columna `pct_vs_dia_normal` de `superposicion_traspaso_caba.csv`). **11 días duplicados, 123.437 transacciones**, marcadas en la tabla de hechos y excluidas de la vista de análisis. El 05/02 solo existe el registro NACIONAL: se reasigna, no se duplica.
- **El 12/02/2026 es el último día de la línea 99:** se fusionó con la 106 como Ramal B (Resolución 18/2026 de la Secretaría de Transporte porteña, publicada el 30/01/2026: https://boletinoficial.buenosaires.gob.ar/normativaba/norma/837365).
- La prensa informa seis líneas eliminadas: la 5 (unida a la 8), la 6 (a la 50), la 23 (a la 115), la 75, la 90 (a la 151) y la 99 (a la 106) (https://www.canal26.com/general/2026/02/05/dos-colectivos-que-recorren-caba-se-fusionaron-la-iconica-linea-que-deja-de-funcionar/).
- **Últimos días en los datos:** línea 6, 07/05/2025; 90, 16/09/2025; 23, 12/11/2025; 75, 30/11/2025; 99, 12/02/2026. La sección 11 los calcula desde los datos y los cruza con la tabla `incidencias_calidad`: 5 de 5 coinciden. La línea 5 no aparece en el período.
- Las líneas 6 y 23 figuran con jurisdicción equivocada **justo en sus últimos días** (MUNICIPAL del 01 al 07/05/2025 y PROVINCIAL del 01 al 12/11/2025), sin registro C.A.B.A. esos días: se reasignan, no se duplican.

**Hallazgo:** los problemas de la fuente que se encontraron se concentran en las transiciones (traspasos, fusiones, bajas).

### Absorción de pasajeros en cada fusión (sección 11)
Método: demanda de la línea receptora en las 4 semanas posteriores frente a la suma de ambas líneas en las 4 semanas previas, corregida por cuánto cambió el resto del sistema en el mismo período (factor estacional). Solo días hábiles sin eventos verificados ni feriados; el último día de servicio, una jornada de transición, queda fuera de las dos ventanas. Todo el cálculo se resuelve en una consulta SQL.

| Fusión | Peso de la línea eliminada | Días hábiles (antes / después) | Factor estacional | Bruta | **Ajustada** | Con medianas | Control sin líneas fusionadas |
|---|---|---|---|---|---|---|---|
| 6 → 50 | 54,3% | 14 / 20 | 0,952 | 80,1% | **84,2%** | 83,6% | 84,2% |
| 23 → 115 | 11,1% | 20 / 17 | 0,990 | 92,4% | **93,4%** | 92,2% | 93,4% |
| 99 → 106 | 45,0% | 20 / 17 | 1,175 | 124,1% | **105,6%** | 105,0% | 105,7% |
| 90 → 151 | 1,4% | 20 / 19 | 1,024 | 109,9% | **107,3%** | 109,1% | 107,2% |

Lectura:
- **Robustas al método:** las dos variantes (medianas en lugar de promedios; grupo de control sin ninguna línea que participe de una fusión) quedan a menos de 2 puntos del método publicado.
- **La ventana previa de la 6 → 50 tiene 14 días hábiles.** Los 6 días hábiles excluidos son eventos verificados o feriados (paro del 10/04, Jueves y Viernes Santo, 1 y 2 de mayo, paro del 06/05), y con medianas el resultado se mueve 0,6 puntos. El control de la sección 11 lo comprueba.
- **La 90 → 151 mide el margen del método.** La 90 pesaba el 1,4% de la 151, así que su cierre casi no mueve a la receptora: el 107% indica que la 151 varió alrededor de un 7% por su cuenta en esas semanas. Es un solo caso, no un margen de error calculado, pero da la escala: el 106% de la 106 no se distingue de retener toda la demanda de la 99, y el 93% de la 115 también queda dentro de ese margen. Solo la 6 → 50 (84%) muestra una pérdida clara: alrededor de 1 de cada 6 viajes (15,8%) no reapareció en la 50.
- **Alcance:** la SUBE da totales por línea y no sigue a las personas. "Absorción" significa que la demanda reapareció en la receptora, no que sean los mismos pasajeros.

---

## 6. ¿Viajamos menos? Tres intentos de refutar la caída (sección 09)

| Prueba | Resultado |
|---|---|
| Cobertura | Colectivo: de 388–394 líneas activas por día hábil en 2024 a 378–381 en 2026 (−3%), en línea con las bajas documentadas. Tren: de 16–18 a 14–16. Subte: 7 (8–9 entre octubre y diciembre de 2025). No explica una caída del 12% |
| Panel balanceado | 381 de 483 líneas, activas en los 32 meses; pesan entre el 96,3% y el 98,1% del total. Caída 2026: −10,9% en el panel contra −11,8% en el total (2025: −3,9% contra −4,3%) |
| Por modo | En 2026 caen los tres: colectivo −11,9%, tren −13,4%, subte −10,7%. Ningún mes mejoró: total entre −8,5% y −20,1%; panel entre −7,8% y −19,3% |
| Mes a mes por modo | Tampoco mejoró ningún modo en ningún mes de 2026. Mejor mes: colectivo −8,0%, tren −11,0%, subte −5,3%. Peor mes: colectivo −22,0%, tren −15,7%, subte −14,2% |
| 2025 frente a 2024 | Total −4,3%; colectivo −2,4%; tren −5,6%; subte −28,1% |

**Qué mide el −11,8%:** el promedio de 8 variaciones mensuales (enero a agosto), cada una con la mediana del día hábil de un mes contra la del mismo mes de 2025. No es la variación del total acumulado.

**Confirmación con el BCRA** (enero–abril, todo el país, sección 10.3):
- Tarjeta SUBE: 1.289,0 M (2025) → 1.080,2 M (2026): **−16,2%**.
- Viajes con QR en 2026: 82,5 M (en enero–abril de 2025, cero).
- Tarjeta + QR: 1.162,7 M → **−9,8%**.
- El QR equivale al 39,5% de lo que perdió la tarjeta (82,5 de 208,8 M).
- Se suman los cuatro meses porque el Carnaval cayó en marzo en 2025 y en febrero en 2026.

### Julio de 2026: el peor mes
- **No es un problema de la fuente:** hubo 378 líneas de colectivo activas por día hábil (379 en junio y agosto), y el panel también cae fuerte (su peor mes es −19,3%).
- **Lo explica sobre todo el colectivo:** su peor mes de 2026 es −22,0%; el del tren, −15,7%.
- **Causa no confirmada:** no se encontró en la prensa un evento específico de julio.

### Alcance: ¿menos viajes o menos colectivos?
- La prensa informó en 2026 una reducción de la oferta de colectivos (iProfesional, 26/04/2026, citando al IIEP de la UBA y el Conicet: alrededor de un 30% menos de vehículos). Es un dato de segunda mano; el informe original no está verificado.
- La SUBE mide viajes realizados: no permite separar si se viaja menos por elección o porque hay menos servicio. Que el tren (−13,4%) y el subte (−10,7%) también caigan sugiere que no es solo un problema de oferta de colectivos. La publicación no atribuye una causa a la caída.

---

## 7. Verificación final

Corrida completa del notebook (`pulso_amba.ipynb`, "Ejecutar todo") con la descarga del 16/10/2026, sin errores:

| Control | Resultado |
|---|---|
| Sección 05 · modelo de datos | 17 de 17 |
| Verificación del post 1 | 26 de 26 |
| Gráfico del post 2 | 11 de 11 |
| Sección 10 · cifras del BCRA (slide 7) | 8 de 8 |
| Gráfico del post 3 (lo dibujado coincide con las cifras) | 5 de 5 |
| Prueba de sensibilidad · escenario A | 25 de 25 |
| Sección 11 · fusiones | verificada |
| **Sección 12 · cada cifra publicada, recalculada** | **66 de 66** |

Además de las cifras, se controlan las frases: "1 de cada 6" exige que 100 dividido la caída redondee a 6; "casi 4 de cada 10", un valor entre 35% y 40%; "colectivo a una quinta parte", un valor entre 18% y 22%; "ningún mes mejoró", que el mejor mes siga siendo negativo.

## Preguntas abiertas

Quedan fuera de la publicación porque los datos disponibles no alcanzan para responderlas:
- ¿El dataset incluye los pagos con QR? Plausible, no demostrado (sección 2).
- ¿Por qué el subte cayó un 28% en 2025?
- ¿Por qué el tren cae a menos de una quinta parte los domingos?
- ¿Qué ramales de tren dejaron de informar (de 16–18 a 14–16 activos por mes)?
- ¿Por qué julio de 2026 cae un 20%?
