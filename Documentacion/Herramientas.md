# REFOOD

## Herramientas
### Tests
### Gestión de productos
Para el desarrollo y ejecución de los tests elaborados, se han empleado las siguientes herramientas o paquetes:

En primer lugar, hemos utilizado [**easygraphql-tester**](https://easygraphql.com/docs/getting-started/overview) que es una librería para Node Js creada para realizar **tests de GraphQL** basados en los **schemas**, que nos permitirá testear tanto estos schemas, como los **resolvers** (queries y mutaciones) que hallamos definido y desarrollado. Podremos comprobar así que las definiciones de tipos, campos requeridos o argumentos sean los correctos, entre otros.

> Esta documentación ha sido elaborada gracias a la documentación oficial. Si desea conocer más acerca del uso o posibilidades de esta librería visite [**EasyGraphQL**](https://easygraphql.com/docs/easygraphql-tester/overview/).

En un segundo nivel superior, hemos utilizado [**Mocha**](https://mochajs.org/) como marco de pruebas que usa un sistema denominado **Behavior Driven Development** o BDD, que nos permitirá definir nuestros tests de una manera descriptiva empleando un lenguaje natural.

Adicionalmente, se han llevado a cabo **tests de cobertura**, que nos permitirán obtener un **porcentaje** relativo al número de **líneas** de nuestro **código testeadas**, lo que nos será de gran utilidad para **comprobar** que partes de nuestro **código** se encuentran **sin testear**. Además nos permite incluir varias opciones como por ejemplo, que el test realizado no resulte satisfactorio a no ser que se supere un mínimo de cobertura (porcentaje de código testeado), o en caso de que esa cobertura se vea decrementada, con lo que **aseguramos** unos **mínimos de calidad**.

Para ello hemos utilizado [**Nyc**](https://github.com/istanbuljs/nyc) como cliente de línea de órdenes para [**Istanbul**](https://istanbul.js.org/) que trabaja bien con **mocha**, y que hemos integrado con [**Codecov**](https://github.com/istanbuljs/nyc/blob/master/docs/setup-codecov.md), que nos permitirá **reportar** los **resultados** del test de **cobertura** a la plataforma, desde la que podremos obtener información de un modo más visual en relación a dichos test realizados.

### Análisis de recetas
En relación a las tecnologías para el desarrollo y ejecución de los **tests** desarrollados para el micro-servicio de **análisis de recetas** elaborado en **python**, se ha empleado en primera instancia el módulo **'testing'** ofrecido por el framework **Falcon**. Este ofrece una serie de funciones que nos permite simular las peticiones sobre una instancia de un módulo API importado.

Para la **ejecución** de dichos **tests**, se ha utilizado **unittest** en un segundo nivel superior, junto con **coverage** al igual que el anterior micro-servicio, para realizar los reportes de cobertura.

Puede consultar [**más información sobre los test**](https://github.com/yoskitar/Cloud-Computing-CC/blob/master/Documentacion/Tests.md) que se han desarrollado.

### Construcción
#### Gestión de productos
buildtool: package.json

#### Package.json
Para el desarrollo de este proyecto se empleará [**package.json**](https://github.com/yoskitar/Cloud-Computing-CC/blob/master/package.json) como **herramienta de construcción**, donde definiremos las **dependencias** necesarias para que nuestro servicio pueda ejecutarse y responder de manera esperada. Además podremos definir una serie de **scripts** como comandos que se ejecutarán al ser llamados empleando la herramienta [**npm**](https://www.npmjs.com/) (node package manager), donde definiremos las órdenes necesarias para ejecutar los tests y generar los reportes mencionados sobre nuestro proyecto.

##### Dependencias
En este archivo encontraremos dos apartados de dependencias:
* `Dependecies`: En esta sección se incluyen las dependencias necesarias y que deben de ser instaladas para que nuestro proyecto funcione correctamente. Por ejemplo `express-graphql`.
* `devDependencies`: En esta entrada se añaden las dependencias necesarias para el desarrollo del proyecto, y que no serán necesarias para el estado de producción. Por ejemplo `mocha`.

Podemos incluir los paquetes o herramientas necesarias en nuestras dependencias empleando las siguientes órdenes:

```
npm install <paquete> --save
```
```
npm install <paquete> --save-dev
```
De esta forma, podremos emplear la siguiente orden para que nuestro proyecto se pueda ejecutar adecuadamente y no se generen conflictos de versiones o de falta de dependecias:
```
npm install 
```
Con esta orden, `npm` instalará aquellas dependecias que hallamos indicado en el package.json. de nuestro proyecto. 

##### Scripts
Como ya hemos mencionado, en esta sección podremos definir órdenes como una secuencia de comandos que podremos ejecutar con npm. En nuestro caso,por ahora solo hemos definido las órdenes **`start`**, **`stop`**, **`restart`** y **`test`**:

```
npm start
```
Esta orden ha sido definida como:
```
pm2-runtime start app/app.js --name \"gp\" -i 2
```
Tras ejecutar esta orden, se lanzarán 2 instancias del micro-servicio definido, accesibles en el puerto `8080` definido internamente como variable de entorno.
* `pm2-runtime`: Gestor de procesos de producción para aplicaciones Node.js con el que levantaremos el micro-servicio.
* `start`: Ejecutará el módulo indicado.
* `-i`: Número de instancias del servicio que se lanzarán.
* `--name`: Nombrará las instancias iniciadas con el nombre indicado. Nos permitirá referirnos facilmente a dichas instancias.

```
npm test 
```

Esta orden ha sido definida como:
```
npx nyc --reporter=lcov mocha app/test/ --exit && npx codecov
```
* `npx`: Viene por defeto con +npm@5.2 y nos permitirá ejecutar los binarios necesarios para lanzar los test, junto a la integración con los test de cobertura con una sola línea, sin necesidad de especificar en el .travis.yml, el uso de un nuevo script para que tras la etiqueta `after_success`, sean lanzados dichos tests de cobertura.
* `nyc`: Cliente de línea de órdenes para Istanbul (indicado arriba).
* `--reporte=lcov`: Establecemos el reportador del test, que en este caso genera el reporte en formato html que puedes visualizar en el navegador.
* `mocha`: Como mencionamos anteriormente, será nuestro marco de pruebas con el que realizaremos los test.
* `--exit`: Finaliza la ejecución del test una vez se han llevado todos a cabo.
* `codecov`: Herramienta que emplearemos para medir la cobertura de nuestro código en relación a los tests realizados.

Con esta orden, se realizarán los test y se enviarán los reportes del test de cobertura adiconal a la plataforma de [**codecov.io**](https://codecov.io/gh/yoskitar/Cloud-Computing-CC).

```
npm stop
```
Esta orden ha sido definida como:
```
pm2-runtime stop gp
```
* `stop`: Detiene las instancias cuyo nombre sea el inidicado en el parámetro (en nuestro caso, 'gp').

Tras ejecutar esta orden, se detendrán las instancias del micro-servicio definido.

```
npm restart
```
Esta orden ha sido definida como:
```
pm2-runtime reload gp
```
* `reload`: Vuelve a iniciar las instancias detenidas con el nombre indicado como parámetro.

Tras ejecutar esta orden, volverán a renaudarse las instancias detenidas del micro-servicio definido.

> Puede consultar el archivo [**package.json**](https://github.com/yoskitar/Cloud-Computing-CC/blob/master/package.json) si aún no lo ha hecho para una mejor comprensión, donde se encuentran los aspectos detallados anteriormente.

#### Servicio completo y análisis de recetas
buildtool: tasks.py

#### Tasks.py
Como herramienta de construcción para el servicio completo, emplearemos tasks.py, donde definiremos las tareas necesarias, que podemos llamar empleando el comando **invoke**.

Las tareas definidas en este archivo son las siguientes:
* Install: Nos permitirá instalar las dependencias necesarias para la ejecución del servicio.
```
npm install --production & pip install requirements.txt
```

* Start: Nos permitirá ejecutar el servicio.
```
npm start & gunicorn -w <nº workers> --threads=<nº threads> --worker-class gevent  -b :<port> --chdir src app:api & python src/analyzer.py
```
* Con la opción --chdir indicamos el directorio donde se encuenta el módulo del API rest.

* Con la opción --worker-class indicamos el tipo de worker, que en este caso se trata de un tipo de worker asincrono (gevent), con el que trataremos de obtener un mejor valor de prestaciones para el servicio.

* Stop: Nos permitirá detener el servicio.
```
npm stop & pkill gunicorn & pkill python
```
* Test: Nos permitirá ejecutar los test y subir los reportes de cobertura del servicio.
```
npm test & coverage run -m unittest src/test/app_test.py
```

Adicionalmente, para cada una de estas tareas definidas, se podrá indicar una serie de parámetros:
* --ms: Nos permitirá especificar el micro-servicio sobre el que ejecutar la tarea seleccionada.
* -p: Para la tarea 'Start', nos permitirá indicar el puerto sobre el que lanzará gunicorn el micro-servicio de análisis de recetas.
* -w: Indicar el número de workers con el que se ejecutará gunicorn.
* -t: Indicar el número de hebras que procesaran las peticiones.

> Puede consultar el archivo [**tasks.py**](https://github.com/yoskitar/Cloud-Computing-CC/blob/master/tasks.py) si aún no lo ha hecho para una mejor comprensión, donde se encuentran los aspectos detallados anteriormente.