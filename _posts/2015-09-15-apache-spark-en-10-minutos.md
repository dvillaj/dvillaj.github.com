---
layout:     post
title:      Apache Spark en 10 minutos 
date:       2015-09-15 11:21:29
summary:    Prueba Apache Spark mediante de una máquina virtual en 10 minutos
categories: BigData Spark
permalink:  /Apache-Spark-10-minutos/
minutes:    10
---

Hola, os dejo una guía paso a paso para poder probar [Apache Spark](http://spark.apache.org/) a partir de la instalación de una máquina virtual en vuestro equipo. Los requisitos mínimos son los siguientes:

*   Un equipo con más de 2Gb de memoria
*   Sistema operativo Windows, Linux o Mac
*   [VirtualBox](https://www.virtualbox.org/)
*   Una [máquina virual](https://goo.gl/B0SxcO) con todo el software necesario ya configurado.
*   Ganas de explorar lo desconocido ;-)

Ahí vamos... El primer paso es descargar la aplicación VirtualBox (https:/www.virtualbox.org), en el caso de que no se tenga previamente instalada en el equipo.

![png](/images/apache-spark-en-10-minutos/guia_spark_1.png) 

![png](/images/apache-spark-en-10-minutos/guia_spark_2.png)

Una vez descargado el fichero, se debe de instalar VirtualBox en el equipo y ejecutarlo (necesitareis permisos de administrador).

![png](/images/apache-spark-en-10-minutos/guia_spark_3.png)

Una vez en VirtualBox, hay que importar el fichero [sparkvm.ova](https://goo.gl/B0SxcO), desde la opción “Importar servicio virtualizado” del menú Archivo: 

![png](/images/apache-spark-en-10-minutos/guia_spark_4.png)

![png](/images/apache-spark-en-10-minutos/guia_spark_5.png) 

![png](/images/apache-spark-en-10-minutos/guia_spark_6.png)

Cuando finalice la importación, se tiene pulsar el botón “Iniciar” para ejecutar la máquina virtual y probar **Spark**. 

![png](/images/apache-spark-en-10-minutos/guia_spark_7.png)

Con la opción "Inicio sin pantalla" es suficiente, ya que la forma de probar Spark es a través del navegador, y no necesitamos tener la consola activa. 

![png](/images/apache-spark-en-10-minutos/guia_spark_32.png)

Se abre un navegador y se accede a la dirección [http://localhost:8001](http://localhost:8001), donde está desplegado [Jupyter](http://jupyter.org/) sobre la máquina virtual que acabamos de instalar. 

![png](/images/apache-spark-en-10-minutos/guia_spark_9.png)

[Jupyter](http://jupyter.org/) es un software donde se pueden editar y ejecutar Notebooks de [IPython](https://en.wikipedia.org/wiki/IPython) o python interactivo. Un Notebook es un documento que contiene un conjunto de celdas. Cada celda puede tener texto, en formato MarkDown, o en código Python que se puede ejecutar de forma individual. Cuando se ejecuta el código contenido en una celda, el resultado se muestra en el propio notebook, justo debajo de la celda. Por ejemplo si introducimos en una celda el código python [_print 1+ 1_] y lo ejecutamos, el resultado será ... 

![png](/images/apache-spark-en-10-minutos/guia_spark_33.png) 

Para probar **Spark** tenemos a descargar el notebook [spark_tutorial_student.ipynb](https://github.com/spark-mooc/mooc-setup/blob/master/spark_tutorial_student.ipynb) que se encuentra disponible en el repositorio [mooc-setup](https://github.com/spark-mooc/mooc-setup) de GitHub. Para descargar este fichero en nuestro disco lo más fácil es a través de la opción de "Guardar enlace como ..."

![png](/images/apache-spark-en-10-minutos/guia_spark_30.png)

Una vez [Jupyter](http://localhost:8001), ya podemos cargar este notobook pulsando el botón upload ... 

![png](/images/apache-spark-en-10-minutos/guia_spark_10.png)

Y volvemos a pulsar el botón Upload, está vez en color azul:

![png](/images/apache-spark-en-10-minutos/guia_spark_11.png)

Solo nos queda hacer doble click sobre el notebook para que aparezca una nueva ventana: 

![png](/images/apache-spark-en-10-minutos/guia_spark_12.png) 

![png](/images/apache-spark-en-10-minutos/guia_spark_13.png) 

Para seguir el tutorial de **Spark** se deben de ejecutar cada una de las celdas de este notebook, leyendo atentamente los textos explicativos. Para ejecutar una celda, primero hay que seleccionarla haciendo click sobre ella … (la celda se muestra con un recuadro verde)

![png](/images/apache-spark-en-10-minutos/guia_spark_14.png)

Y pulsar el botón de ejecución ...

![png](/images/apache-spark-en-10-minutos/guia_spark_15.png)  

Hasta que lleguemos al final... También se pueden ejecutar otros notebooks de este [repositorio](https://github.com/spark-mooc/mooc-setup), aunque la mayoría requiere que introduzcamos código en python. En especial es muy interesante hacer los notebook ‘lab0_student.ipynb’ y ‘lab1_word_count_student.ipynb’. El resto es un poco más complicado pero os animo a que lo intentéis. Por último, comentaros que para cerrar la máquina virtual simplemente se deberá ejecutar la opción "Apagar" o pulsar Control-F

![png](/images/apache-spark-en-10-minutos/guia_spark_31.png)

Espero que lo hayáis encontrado interesante...
