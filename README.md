# lab3-IO_performance-IsabellaSanchezMejia


![alt text](<Banner para Linkedin programador con fotografia azul y blanco (2).png>)


<div>
  <img style="width:100%" src="https://capsule-render.vercel.app/api?type=waving&height=100&section=header&fontSize=70&fontColor=FFFFFF&color=87CEEB" />
</div>

## 1. Especificaciones del equipo

| Parámetro                         | Valor Observado                          |
|----------------------------------|------------------------------------------|
| Sistema Operativo                | Windows 11                               |
| CPU (Modelo y Frecuencia)        | Intel Core i5-13420H @ 2.10 GHz          |
| Arquitectura y Núcleos           | x64 / 8 núcleos                          |
| Memoria RAM Total                | 8 GB                                     |
| Tecnología de Almacenamiento     | SSD NVMe                                 |
| Carga de CPU en Reposo (%)       | ~1%                                      |

<img src="https://user-images.githubusercontent.com/73097560/115834477-dbab4500-a447-11eb-908a-139a6edaec5c.gif"><br><br>
## 2. Análisis y resultado del experimento


1.**Comparación de patrones:** *Con base en sus mediciones, ¿cuántas
   veces más rápido fue el acceso secuencial respecto al aleatorio en
   su equipo? ¿Ese resultado era el esperado según la teoría?*


En mi equipo, el acceso secuencial demostró ser significativamente más eficiente que el acceso aleatorio, especialmente en tamaños de bloque pequeños. Por ejemplo, para bloques de 4 KiB, la tendencia general muestra que el acceso secuencial se estabiliza rápidamente y alcanza un mayor rendimiento.

> **Importante:**
> En términos de throughput, el acceso secuencial fue aproximadamente hasta 1.6 veces más rápido que el acceso aleatorio.

Este resultado coincide con lo esperado según la teoría, ya que el acceso secuencial permite al sistema operativo  realizar técnicas como la lectura anticipada, lo que reduce el impacto de la latencia. En contraste, el acceso aleatorio implica saltos constantes entre ubicaciones de memoria, lo que incrementa y "dispara" los tiempos de acceso y disminuye el rendimiento.

![alt text](image.png)
<img src="https://user-images.githubusercontent.com/73097560/115834477-dbab4500-a447-11eb-908a-139a6edaec5c.gif"><br><br>

2.**Efecto del tamaño de bloque:** *¿Qué ocurrió con el throughput del
   acceso aleatorio a medida que aumentó el tamaño de bloque?
   ¿Por qué cree que sucede eso?*

A medida que el tamaño del bloque aumenta (de 4 KiB a 256 KiB), el throughput (MiB/s) de ambos tipos de acceso crece considerablemente, hasta el punto de casi igualarse en los 256 KiB.

Esto sucede porque, al trabajar con bloques más grandes, la latencia asociada a cada solicitud se “suaviza”. Es decir, el sistema pasa menos tiempo gestionando la operación (como la búsqueda o posicionamiento) y más tiempo transfiriendo datos útiles.

En bloques pequeños, esa latencia tiene un impacto alto, especialmente en el acceso aleatorio. Sin embargo, cuando el tamaño del bloque alcanza valores como 256 KiB, esa sobrecarga se vuelve casi insignificante frente a la cantidad de datos transferidos. Por esta razón, el acceso aleatorio logra acercarse mucho al rendimiento del secuencial, alcanzando velocidades cercanas a los 5000 MiB/s.

![alt text](images/fig_throughput.png)
<img src="https://user-images.githubusercontent.com/73097560/115834477-dbab4500-a447-11eb-908a-139a6edaec5c.gif"><br><br>

3.**Teoría vs práctica:** *Identifique un caso en sus resultados donde
   la medición empírica se alejó del modelo teórico. ¿A qué factor
   atribuye esa diferencia?*

Al comparar el tiempo empírico con el modelo teórico, se pueden identificar dos comportamientos importantes:

En el acceso aleatorio, el tiempo empírico resulta menor que el teórico. Esto se debe principalmente al uso de caché de lectura y a los algoritmos de optimización del sistema operativo, que ayudan a reducir el impacto real de las peticiones y “suavizan” su costo.

Por otro lado, en el acceso secuencial, especialmente con bloques muy pequeños (como 4 KiB), el tiempo empírico aumenta considerablemente y supera al teórico.

**Importante:**
> Esto ocurre debido a la sobrecarga de las llamadas al sistema. Cuando se procesan muchas operaciones pequeñas, el sistema debe gestionar una gran cantidad de solicitudes, lo que genera un cuello de botella en la CPU. 
 
 Además, la gestión interna del sistema operativo y el uso de caché también pueden influir en el comportamiento real del sistema. Ya que estos elementos no son tenidos en cuenta en los modelos teóricos, esto explica la razón de la discrepancia observada en los resultados.
 

![alt text](images/fig_tiempo_teoria_vs_practica_aleatorio.png)
![alt text](images/fig_tiempo_teoria_vs_practica_secuencial.png)

<img src="https://user-images.githubusercontent.com/73097560/115834477-dbab4500-a447-11eb-908a-139a6edaec5c.gif"><br><br>

4.**Tipo de disco:** *Compare sus resultados con los valores de referencia
   de la tabla de la guía. ¿Su equipo se comportó como un HDD, un SSD
   SATA o un SSD NVMe?*


Tabla de referencia:

| Tecnología | Latencia Promedio | Throughput Típico | IOPS Típico (4 KB aleatorio) | Escala de Tiempo |
| --- | --- | --- | --- | --- |
| **HDD** | 10 ms | 100 - 150 MB/s | 75 – 300 | Milisegundos |
| **SSD (SATA)** | 100 µs | 500 - 550 MB/s | 50,000 – 100,000 | Microsegundos |
| **SSD NVMe** | 10 - 20 µs | 2 - 7 GB/s | 500,000 – 1,000,000+ | Microsegundos |

Con base en la comparación con la tabla de referencia, mi equipo se comporta como un SSD NVMe. Esto se debe a que el throughput obtenido en las pruebas alcanza valores cercanos a 5000 MiB/s (≈ 5 GB/s), lo cual se encuentra dentro del rango típico de un SSD NVMe (2 – 7 GB/s). Además, estos valores están muy por encima de los observados en un HDD (100 – 150 MB/s) y un SSD SATA (500 – 550 MB/s).

Adicionalmente, el comportamiento del acceso aleatorio muestra una penalización mínima en bloques grandes, lo que coincide con las características de baja latencia (10 – 20 µs) y alto rendimiento en IOPS propias de los SSD NVMe.

Por lo tanto, los resultados experimentales son coherentes con los valores teóricos presentados en la tabla y confirman que el sistema utiliza tecnología NVMe.

<img src="https://user-images.githubusercontent.com/73097560/115834477-dbab4500-a447-11eb-908a-139a6edaec5c.gif"><br><br>

5.**Aplicación práctica:** *Imagine que debe almacenar una tabla de
   estudiantes con 1 millón de registros. Con base en lo que midió,
   ¿preferiría leerla toda de forma secuencial o acceder a registros
   individuales de forma aleatoria? ¿Por qué?*

Si se necesita procesar un millón de registros para generar un reporte completo, la opción que elegiría sería la secuencial. Según los resultados obtenidos, el acceso secuencial mantiene una latencia baja y llega a estabilizarse (aplanarse) cuando se trabaja con bloques de tamaño considerable. Como se observa en la imagen, el rendimiento del acceso secuencial se vuelve más constante a medida que aumenta el tamaño del bloque. En este caso, los datos se leen de forma continua, lo que permite aprovechar mejor el rendimiento del sistema.

En cambio, realizar esta misma tarea mediante acceso aleatorio implicaría gestionar un millón de ubicaciones distintas en memoria. Aunque se trate de un SSD rápido, esto genera una sobrecarga innecesaria y reduce la eficiencia, ya que el sistema no puede optimizar la lectura.

![alt text](image-1.png)

<img src="https://user-images.githubusercontent.com/73097560/115834477-dbab4500-a447-11eb-908a-139a6edaec5c.gif"><br><br>
## Conclusión final


La información en el almacenamiento se organiza en bloques o también conocidos como sectores físicos; es vital entenderlo porque el hardware está optimizado para leer datos contiguos de una sola vez. Aunque en un SSD no hay un cabezal físico moviéndose, el acceso secuencial sigue siendo mejor que el aleatorio porque permite al controlador de memoria realizar lecturas anticipadas. En mis pruebas, identifiqué que esta diferencia es crítica en bloques pequeños, donde registré un factor de mejora (speedup) máximo de 1.6x a los 16 KiB. El modelo teórico predijo con gran acierto el comportamiento de mi equipo, demostrando que a medida que el tamaño del bloque aumenta, el tiempo de transferencia domina sobre la latencia de búsqueda, haciendo que el factor M tienda a 1.En un sistema real, mi decisión de diseño sería implementar estructuras de datos que estén orientadas al acceso secuencial,ya que permitiría que aunque los datos lleguen dispersos, el sistema los organice en memoria para escribirlos al disco de forma ordenada.

<div>
  <img style="width:100%" src="https://capsule-render.vercel.app/api?type=waving&height=100&section=footer&fontSize=70&fontColor=FFFFFF&color=87CEEB" />
</div>
