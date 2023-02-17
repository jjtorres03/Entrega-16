# Desafío 16

## Loggers, Gzip y análisis de performance.

### Consignas

Incorporar al proyecto de servidor de trabajo la compresión gzip.

Verificar sobre la ruta /info con y sin compresión, la diferencia de cantidad de bytes devueltos en un caso y otro.

Luego implementar loggueo (con alguna librería vista en clase) que registre lo siguiente:

   - Ruta y método de todas las peticiones recibidas por el servidor (info)
   - Ruta y método de las peticiones a rutas inexistentes en el servidor (warning)

Errores lanzados por las apis de mensajes y productos, únicamente (error)
Considerar el siguiente criterio:
  - Loggear todos los niveles a consola (info, warning y error)
  - Registrar sólo los logs de warning a un archivo llamada warn.log
  - Enviar sólo los logs de error a un archivo llamada error.log

----

Luego, realizar el análisis completo de performance del servidor con el que venimos trabajando.

Vamos a trabajar sobre la ruta '/info', en modo fork, agregando ó extrayendo un console.log de la información colectada antes de devolverla al cliente. Además desactivaremos el child_process de la ruta '/randoms'

Para ambas condiciones (con o sin console.log) en la ruta '/info' OBTENER:

  - El perfilamiento del servidor, realizando el test con --prof de node.js. Analizar los resultados obtenidos luego de procesarlos con --prof-process. 
  - Utilizaremos como test de carga Artillery en línea de comandos, emulando 50 conexiones concurrentes con 20 request por cada una. Extraer un reporte con los resultados en archivo de texto.

----

Luego utilizaremos Autocannon en línea de comandos, emulando 100 conexiones concurrentes realizadas en un tiempo de 20 segundos. Extraer un reporte con los resultados (puede ser un print screen de la consola)

  - El perfilamiento del servidor con el modo inspector de node.js --inspect. Revisar el tiempo de los procesos menos performantes sobre el archivo fuente de inspección.
  - El diagrama de flama con 0x, emulando la carga con Autocannon con los mismos parámetros anteriores.
  
Realizar un informe en formato pdf sobre las pruebas realizadas incluyendo los resultados de todos los test (texto e imágenes). 

👉 Al final incluir la conclusión obtenida a partir del análisis de los datos.

### Conclusión

Sin console log

```console

╰─>$ node benchmark.js
Running tests
Running 5s test @ http://localhost:3030/test/info
100 connections with 10 pipelining factor

┌─────────┬─────────┬─────────┬─────────┬─────────┬────────────┬───────────┬─────────┐   
│ Stat    │ 2.5%    │ 50%     │ 97.5%   │ 99%     │ Avg        │ Stdev     │ Max     │   
├─────────┼─────────┼─────────┼─────────┼─────────┼────────────┼───────────┼─────────┤   
│ Latency │ 2153 ms │ 3405 ms │ 4708 ms │ 4868 ms │ 3433.09 ms │ 781.39 ms │ 4963 ms │   
└─────────┴─────────┴─────────┴─────────┴─────────┴────────────┴───────────┴─────────┘   
┌───────────┬─────┬──────┬────────┬────────┬────────┬────────┬───────┐
│ Stat      │ 1%  │ 2.5% │ 50%    │ 97.5%  │ Avg    │ Stdev  │ Min   │
├───────────┼─────┼──────┼────────┼────────┼────────┼────────┼───────┤
│ Req/Sec   │ 0   │ 0    │ 330    │ 371    │ 214.4👈  │ 175.28 │ 1   |
├───────────┼─────┼──────┼────────┼────────┼────────┼────────┼───────┤
│ Bytes/Sec │ 0 B │ 0 B  │ 204 kB │ 229 kB │ 132 kB │ 108 kB │ 611 B │
└───────────┴─────┴──────┴────────┴────────┴────────┴────────┴───────┘

Req/Bytes counts sampled once per second.
# of samples: 5

2k requests in 5.09s, 662 kB read
```

👈Todas las pruebas realizadas nos indican que en el caso que logueamos por consola la respuesta antes de ser enviada el tiempo de respuesta es mayor y que en el mismo tiempo, se pueden manejar menos requests. 



