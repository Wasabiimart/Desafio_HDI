# Desafio_HDI

El siguiente documento busca documentar el proceso seguido para realizar el desafio para el puesto de cientifico de datos jr

1. Hay que comenzar con el notebook P1, cuyo objetivo es llegar a las variables que se usaran en el modelo de clasificación de fraudes, para ello se toma carga el dataframe (dataset.csv) con el nombre df, donde se hace un análisis exploratorio de las variables, buscando casos que tengan missings, casos atípicos y valores únicos
2. Se eliminan los casos atípicos de las variables CANTIDAD_HIJOS (999) y ANIO_VEHICULO (2030), se tranforman los valores de la variable ESTADO_CIVIL para que sean iguales entre ellos, se pasan las variables de fechas a formato datetime, se rellenan los missings de las variables numéricas por medio del método knn, se eliminan los casos donde hay missings en las variables FEC_SINIESTRO, PRODUCTO, MARCA_VEHICULO y ESTADO_CIVIL, debido a que no había una forma para rellenarlos de forma consisa y se elimina la variable CANTIDAD_AUTOS debido a su alto porcentajde missings (89%).
3. Se procede a crear las siguientes variables:
   - DIAS_SIN_DEN: días entre el siniestro y la denuncia
   - WEEKDAY_SINIESTRO: día de la semana de la fecha del siniestro
   - WEEKDAY_DENUNCIO: día de la semana de la fecha de denuncio
   - DAY_SINIESTRO: día de la fecha del siniestro
   - DAY_DENUNCIO: día de la fecha de denuncio
   - MONTH_SINIESTRO: mes de la fecha del siniestro
   - MONTH_DENUNCIO: mes de la fecha de denuncio
   - ANT_VEHICULO: diferencia en años entre ANIO_VEHICULO y el año actual (2025)
   - RATIO_PRIMA_DEDUCIBLE: ratio entre PRIMA_MENSUAL_UF y DEDUCIBLE, en caso que el deducible sea 0, se tomara el valor 0
   - PRIMA_POR_ROBO: producto de PRIMA_MENSUAL_UF y ROBO
   - PRIMA_POR_HIJO: producto de PRIMA_MENSUAL_UF y CANTIDAD_HIJOS
   - PRIMA_POR_ANT: producto de PRIMA_MENSUAL_UF y ANT_VEHICULO
   - DEDUCIBLE_POR_ROBO: producto de DEDUCIBLE_MENSUAL_UF y ROBO
   - DEDUCIBLE_POR_HIJO: producto de DEDUCIBLE_MENSUAL_UF y CANTIDAD_HIJOS
   - DEDUCIBLE_POR_ANT: producto de DEDUCIBLE_MENSUAL_UF y ANT_VEHICULO
4. Con respecto a las variables categóricas, se procede a usar el método WoE (Weight of Evidence) para transformarlas a numércias, usando los casos 'Descartado' como bueno y 'Fraude' como malo.
5. Se sigue con el modelo  usando las categorias "Fraude" y "Descartado" y solamente las variable numéricas para usar el modelo Isolation Forest, ya que es ideal para buscar anormalidades en datos, esto debido a que la cantidad de casos catalogados como fraudes es pequeña (1%)
6. Se usan varios valores para los parámetros del modelo (0.01, 0.02, 0.0305, 0.04, 0.05 y 0.1 para contamination y 100, 200, 300, 400, 500, 750 y 1000 para n_estimators) y siempre la semila 123 para asegurar replicabilidad.
7. Del mejor modelo resultante se buscó obtener la importancia de las variables, donde luego de un proceso iterativo, las que dieron mejor resultado fueron ROBO, MONTH_DENUNCIO_WOE, WEEKDAY_DENUNCIO_WOE, CANAL_CONTRATACION_WOE, DEDUCIBLE_POR_HIJO, DAY_DENUNCIO_WOE, PRIMA_POR_ROBO, CANTIDAD_HIJOS, PRIMA_POR_HIJO, MONTH_SINIESTRO_WOE y DEDUCIBLE_POR_ROBO, además que en otro proceso iterativo se llego a los valores 0.04 y 750 para contamination y n_estimators, respectivamente, ya que son los que alcanzaban los mejores rendimientos del modelo y se acercaban al caso real
8. Se prosiguió con el notebook P2, donde se replica lo anterior, solamente que con las variables finales definidas
9. Ya una vez obtenido un modelo entrenado y validado, se procede a realizar las predicciones los casos catalogados como "No revisado", donde se llego a catalogar 739 casos como fraudes.
10. Ya realizado esto, se prosigue calculando el valor del monto fraude, donde se utilizó el metodo knn debido a que eran pocos los casos para entrenar.
11. Finalmente, se llego a tener 739 casos catalogados como fraudes, lo que se traduciría como una tasa de fraude del 4.65% con respecto al total de casos "No revisado" y con un monto de fraude 17.550.999
12. De igual forma, como recomendación futura hacia el proceso y resultados obtenidos, se podría ampliar la cantidad de variables, con tal de robustecer el modelo final y buscar otros métodos para el modelamiento y procedimiento que podrían hacer más ágil el proceso
