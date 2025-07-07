# S11---ALIMENTOS_PRUEBA_AAB
Prueba A/A/B para cambio de tipografía en aplicación de empresa de productos alimenticios. Python.

## Descripción

Al equipo de diseño de una empresa de productos alimenticios le gustaría cambiar las fuentes de toda la aplicación , pero la gerencia teme que los usuarios piensen que el nuevo diseño es intimidante. Por ello, deciden realizar un test A/A/B.

Los usuarios se dividen en tres grupos: dos grupos de control obtienen las fuentes antiguas y un grupo de prueba obtiene las nuevas. ¿Qué conjunto de fuentes produce mejores resultados?

## Herramientas utilizadas
![Python](https://img.shields.io/badge/:Python-024A86?style=for-the-badge&logo=python&logoColor=white&labelColor=101010)</br>
![Matplotlib](https://img.shields.io/badge/Matplotlib-%23ffffff.svg?style=for-the-badge&logo=Matplotlib&logoColor=black)
![NumPy](https://img.shields.io/badge/numpy-%23013243.svg?style=for-the-badge&logo=numpy&logoColor=white)
![Pandas](https://img.shields.io/badge/pandas-%23150458.svg?style=for-the-badge&logo=pandas&logoColor=white)
![Seaborn](https://img.shields.io/badge/seaborn-%233F4F75.svg?style=for-the-badge&logo=seaborn&logoColor=white)
![SciPy](https://img.shields.io/badge/SciPy-%230C55A5.svg?style=for-the-badge&logo=scipy&logoColor=%white)
![Datetime](https://img.shields.io/badge/datetime-%233F4F75.svg?style=for-the-badge&logo=datetime&logoColor=white)

![Colab](https://img.shields.io/badge/Colab-F9AB00?style=for-the-badge&logo=googlecolab&color=525252)
![Jupyter Notebook](https://img.shields.io/badge/jupyter-%23FA0F00.svg?style=for-the-badge&logo=jupyter&logoColor=white)

## Conclusiones
Los cambios en las fuentes de la página web no han surtido efecto alguno en el comportamiento de los usuarios.
No hemos obtenido diferencias significativas entre los grupos.

## Profundización técnica
* Exploración de datos:
* * Lectura de datasets
  * Correción de nombres de columnas
  * Correción de tipos de datos
  * * Fechas en Unix Time a timestamp :
    * * Fecha tipo objeto - data['event_timestamp'].map(lambda x: dt.datetime.utcfromtimestamp(x).strftime('%Y-%m-%d %H:%M:%S'))
      * Fecha tipo datetime - data['event_timestamp'].map( lambda x: dt.datetime.strptime(x, '%Y-%m-%d %H:%M:%S'))
  * Manejo de valores ausentes
* Análisis - comparación de los eventos en cada uno de los grupos
* * ¿Cuántos y cuáles son los eventos registrados? events.unique()
  * ¿Cuál es el promedio de eventos por usuario?   events.count()/device_id.nunique()
  * ¿Qué periodo de fechas cubren los datos?      event_timestamp.min(), event_timestamp.max()
  * *  ¿Qué periodo representan realmente los datos?  -  Histograma por fecha y hora:  plt.hist(event_timestamp,bins=#)
    *  ¿Se perdieron muchos datos al excluir los más antiguos?
  * Asegurarse de tener los usuarios de los tres grupos.
* Análisis EMBUDOS:
* * Embudo de ventas - ¿Todas las acciones son secuenciales?¿Cuántas y cuáles son las secuencias de eventos?
  * * Frecuencia por evento, uso de diccionarios - dict.update({key:value}) , dict(sorted(events_freq.items(), key=lambda item: item[1],reverse= True))
    * Usuarios por evento - tabla pivote:  index=device_id, columns= events , value=event_timestamp, aggfunc=min ;  filtros eventos realizados en orden cronologico.
    * Proporción de usuarios por evento con respecto a paso anterior - (user_current_event * 100) / user_last_event
    * Gráfica de embudo - plt.style.use("dark_background"), sns.barplot(x='events', y='event_user_proportion', data=funnel_data), plt.xlabel(''),plt.ylabel(''), plt.title(''), plt.xticks(rotation=90), plt.show()
    * ¿En qué etapa se pierden más usuarios?
    * ¿Qué porcentaje de usuarios hace todo el viaje, desde primera etapa hasta el pago? - (users_pay * 100)/users_firststep
* Análisis estadístico - Test A/A/B:
* * ¿Cuántos registros y usuarios únicos hay por grupo? ¿Los grupos son te tamaño similar?¿Los resgistros se reparten equitativamente entre los grupos?
  * Aplicación de Método Mann Withney comparando cada evento entre los grupos A1-A2, A1-B, A2-B
  * Elección de valor de alfa de acuerdo a método de Bonferroni - alfa_corregidda = alfa_requerida / #_prueba 
