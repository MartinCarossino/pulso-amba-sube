# Cifras publicadas

Cada cifra de la serie, su valor calculado y dónde se recalcula en `pulso_amba.ipynb`. Descarga de referencia: 16/10/2026 (1.517.680 filas); período consolidado hasta el 31/08/2026.

**Cómo leer la columna "Control".** La sección 12 recalcula las cifras agrupadas como "slide 1" a "slide 10", una numeración de trabajo de la verificación: slides 2 a 5, post 1; slide 6, post 2; slide 7, post 3; slides 8 y 9, post 4; slides 1 y 10, cifras generales y datos complementarios. La tolerancia es la del redondeo: si se publica 84%, vale cualquier valor entre 83,5 y 84,5.

---

## Post 1 · Un paro de colectivos deja al AMBA como un domingo

100 = día hábil típico (mediana de los días hábiles sin feriados, paros ni eventos, AMBA).

| Cifra publicada | Valor calculado | Origen | Control |
|---|---|---|---|
| 10,5 millones de viajes en un día hábil típico | 10,50 M | Sección 08 | Slide 2 |
| Más de 8 de cada 10 viajes en colectivo (84%) | 84,35% (tren 10,14%; subte 5,50%) | Sección 08.1 | Slide 2 |
| Sábado 60 | 60,1 | Sección 08.2 | Slide 3 |
| Domingo 31 | 31,1 | Sección 08.2 | Slide 3 |
| Paro de trenes y subte, con colectivos (10/04/2025): 53 | 53,1 | Vista `vista_dia_vs_habil_tipico` | Slide 5 |
| Feriado nacional: 35 (entre 27 y 42) | Mediana 35,1; rango 27,3–42,3 (27 feriados) | Sección 07.1 | Slide 4 (rango) y prueba de sensibilidad (mediana) |
| Paro de colectivos: 30 (mediana de 27 y 33) | 30,2 (06/05/2025: 27,3; 11/04/2024: 33,0) | Vista `vista_dia_vs_habil_tipico` | Slide 5 y prueba de sensibilidad |
| Paro general: 6 | 6,4 (09/05/2024: 4,8; 19/02/2026: 8,0) | Vista `vista_dia_vs_habil_tipico` | Slide 5 y prueba de sensibilidad |
| 1,5 millones de registros | 1.517.680 | Sección 01 | Slide 1 |

## Post 2 · De enero a agosto de 2026, el AMBA viajó un 12% menos

Promedio de las variaciones de enero a agosto, cada mes contra el mismo mes de 2025 (mediana del día hábil, AMBA).

| Cifra publicada | Valor calculado | Origen | Control |
|---|---|---|---|
| 12% menos; quedan 88 de cada 100 | −11,8% | Sección 09.3 | Slide 6 y gráfico del post 2 |
| Colectivo −11,9% / tren −13,4% / subte −10,7% | −11,9 / −13,4 / −10,7 | Sección 09.3 | Slide 6 y gráfico del post 2 |
| Los ocho meses por debajo de 2025, entre −8,5% y −20% | −8,5 a −20,1 | Sección 09.3 | Slide 6 |
| 381 líneas con viajes todos los meses (32 meses), 96–98% del total | 381 de 483; 96,3–98,1% | Sección 09.2 | Slide 6 y gráfico del post 2 |
| Panel: −10,9% | −10,9 | Sección 09.3 | Slide 6 y gráfico del post 2 |

## Post 3 · La tarjeta SUBE perdió 1 de cada 6 viajes; el QR compensó casi 4 de cada 10

Banco Central, enero a abril, todo el país (`datos_bcra.csv`).

| Cifra publicada | Valor calculado | Origen | Control |
|---|---|---|---|
| Tarjeta SUBE: 1.289 → 1.080 millones de viajes (−16%) | 1.289,0 → 1.080,2 M; −16,2% | Sección 10.3 | Sección 10.4 y slide 7 |
| 1 de cada 6 viajes | 100 / 16,2 = 6,17 | Sección 10.3 | Sección 10.4 y slide 7 |
| 83 millones de viajes con QR en 2026 | 82,5 M | Sección 10.3 | Sección 10.4 y slide 7 |
| 209 millones que perdió la tarjeta | 1.289,0 − 1.080,2 = 208,8 M | Sección 10.3 | Sección 10.4 |
| Casi 4 de cada 10 | 82,5 / 208,8 = 39,5% | Sección 10.3 | Sección 10.4 y slide 7 (entre 35% y 40%) |
| Tarjeta + QR: −10% | −9,8% | Sección 10.3 | Sección 10.4 y slide 7 |
| Ningún viaje con QR en enero–abril de 2025 | 0 (el Viaje con QR empezó el 12/05/2025) | `datos_bcra.csv` | Sección 10.3 |

## Post 4 · Lo que no se ve (carrusel de verificación)

| Cifra publicada | Valor calculado | Origen | Control |
|---|---|---|---|
| 123.437 viajes contados dos veces | 123.437 transacciones marcadas como duplicadas | Sección 04.6 | Sección 05 (grupo D) |
| La 99, en 11 de sus últimos 12 días | 11 días duplicados, del 01/02 al 12/02/2026 sin el 05/02 | Sección 04.6 e incidencias 2 y 3 | Sección 05 y slide 9 |
| La suma llegó a más del doble de un día normal | Máximo 281,6%; 10 días por encima del 200% | Sección 06.3 (`superposicion_traspaso_caba.csv`) | — |
| 31 líneas pasaron de Nación a la Ciudad | 31, idénticas al acta | Sección 06.2 | Sección 05 y slide 9 |
| En los datos, el cambio aparece el 01/02/2025 | Corte en el sistema: 01/02/2025 | Sección 06.3 e incidencia 1 | — |
| La 6 y la 23 figuran como municipal y provincial en sus últimos días | 6: 01–07/05/2025 (MUNICIPAL); 23: 01–12/11/2025 (PROVINCIAL) | Incidencias 4, 6, 7 y 8 | — |
| Cinco líneas dejaron de figurar entre mayo de 2025 y febrero de 2026 | 6, 90, 23, 75 y 99 (del 07/05/2025 al 12/02/2026) | Sección 11 e incidencias FIN_DE_SERVICIO | Sección 11 (5 de 5) y slide 9 |
| 99 → 106: 106% | 105,6% (variantes 105,0–105,7) | Sección 11 | Sección 11 y slide 8 |
| 23 → 115: 93% | 93,4% (variantes 92,2–93,4) | Sección 11 | Sección 11 y slide 8 |
| 6 → 50: 84%; 1 de cada 6 viajes no reapareció | 84,2%; 100 / 15,8 = 6,3 | Sección 11 | Sección 11 y slide 8 |
| Margen del método: la 90 pesaba el 1,4% de la 151, que dio 107% | 1,4%; 107,3% | Sección 11 | Sección 11 y slide 8 |
| 17 controles automáticos | 17 de 17 | Sección 05 | Slide 9 |
| Entre −2% y −8% con el Banco Central (enero–octubre 2025) | −2,1 a −8,4 | Sección 10.2 | Slide 9 |
| 44 de 44 feriados en día hábil muestran caída | 44; entre 13,8% y 62,6% | Sección 07.1 | Slide 4 |
| La caída de 2026 se mueve como mucho 0,2 puntos sin los 7 días con caída sin causa | 0,16 puntos (escenario B) | Prueba de sensibilidad | Escenario A, 25 de 25 |
| 66 controles finales | 66 de 66 | Sección 12 | — |

## Datos complementarios (slide 10)

| Cifra | Valor calculado | Origen | Control |
|---|---|---|---|
| El domingo, el tren pierde más de 4 de cada 5 viajes | 18,6% de un día hábil | Sección 08.3 | Slide 10 |
| El colectivo conserva un tercio el domingo | 32,7% | Sección 08.3 | Slide 10 |
| Subte: −28% en 2025 frente a 2024 | −28,1% | Sección 09.3 | Slide 10 |
| Julio de 2026, el peor mes: −20% | −20,1%; 378 líneas de colectivo activas por día hábil (379 en junio y agosto) | Secciones 09.1 y 09.3 | Slide 6 (−20%); las líneas, en la tabla de la 09.1 |
