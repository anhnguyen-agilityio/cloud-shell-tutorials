# Instructivo sobre el conteo de palabras de Dataflow

<walkthrough-tutorial-url url="https://cloud.google.com/dataflow/docs/quickstarts/quickstart-python"></walkthrough-tutorial-url>
<walkthrough-watcher-constant value="dataflow-intro" key="directory"></walkthrough-watcher-constant>
<walkthrough-watcher-constant value="dataflow-intro" key="job-name"></walkthrough-watcher-constant>

## Introducción

En este instructivo, ejecutarás un simple ejemplo de canalización con Python para familiarizarte con los conceptos básicos del servicio de Cloud Dataflow.

Las canalizaciones de Dataflow pueden ser por *lotes* (que procesan entradas delimitadas, como un archivo o una tabla de base de datos) o de *transmisión* (que procesan entradas no delimitadas de una fuente, como Cloud Pub/Sub). El ejemplo que se brinda en este instructivo corresponde a una canalización por lotes que cuenta palabras en una colección de obras de Shakespeare.

Antes de comenzar, debes revisar el proyecto de Cloud Platform para saber si cumples con los requisitos previos y realizar la configuración inicial.

## Configuración del proyecto

Google Cloud Platform organiza los recursos en proyectos. Esto te permite recopilar todos los recursos relacionados con una aplicación específica en un solo lugar.

<walkthrough-project-billing-setup></walkthrough-project-billing-setup>
<walkthrough-project-permissions permissions="dataflow.jobs.create"></walkthrough-project-permissions>

## Configura Cloud Dataflow

Para usar Dataflow, activa las API de Cloud Dataflow y abre Cloud Shell.

### Activa las API de Google Cloud

<walkthrough-enable-apis apis="compute.googleapis.com,dataflow,cloudresourcemanager.googleapis.com,logging,storage_component,storage_api,bigquery,pubsub">
</walkthrough-enable-apis>

### Abre Cloud Shell

Cloud Shell es una herramienta de línea de comandos integrada para Console. Usarás Cloud Shell para implementar tu app.

Para abrir Cloud Shell, haz <walkthrough-cloud-shell-icon></walkthrough-cloud-shell-icon>clic en el [ícono][spotlight-open-devshell] de la barra de navegación ubicada en la parte superior de Console.

## Instala muestras de Cloud Dataflow en Cloud Shell

La versión de Python de Cloud Dataflow necesita un entorno de programación con Python, el SDK de Google Cloud y el SDK de Cloud Dataflow para Python.
Además, Cloud Dataflow usa pip, el administrador de paquetes de Python, para administrar las dependencias del SDK.

En este instructivo, se usa una versión de Cloud Shell que incluye Python y pip instalados. Si lo deseas, puedes hacer este instructivo [en tu máquina local][dataflow-python-tutorial].

### Descarga las muestras y el SDK de Cloud Dataflow para Python con el comando de pip

Cuando ejecutes el siguiente comando, se descargará pip y se instalará la versión adecuada del SDK de Cloud Dataflow.

```bash
pip install --user google-cloud-dataflow
```

Ejecuta el comando de pip en Cloud Shell.

## Configura un depósito de Cloud Storage

Cloud Dataflow usa depósitos de Cloud Storage para almacenar datos de salida y almacenar en caché tu código de canalización.

### Ejecuta gsutil mb

En Cloud Shell, usa el comando `gsutil mb` para crear un depósito de Cloud Storage.

```bash
gsutil mb gs://{{project-id-no-domain}}
```

Para obtener más información sobre la herramienta `gsutil`, consulta la [documentación][gsutil-docs].

## Crea y lanza una canalización

En Cloud Dataflow, el trabajo de procesamiento de datos se representa mediante una *canalización*. Una canalización lee datos de entrada, les aplica transformaciones y, luego, genera datos de salida. Las transformaciones de una canalización pueden incluir operaciones de filtrado, agrupamiento, comparación o combinación.

### Lanza tu canalización en el servicio de Dataflow

Usa Python para iniciar tu canalización en el servicio de Cloud Dataflow. La canalización en ejecución recibe el nombre de *trabajo*.

```bash
python -m apache_beam.examples.wordcount \
  --project {{project-id}} \
  --runner DataflowRunner \
  --temp_location gs://{{project-id-no-domain}}/temp \
  --output gs://{{project-id-no-domain}}/results/output \
  --job_name {{job-name}}
```

  *  `output` es el depósito que el ejemplo de Conteo de palabras usa para almacenar los resultados del trabajo.

### Tu trabajo se está ejecutando

Felicitaciones. Tu ejecutable ya se organizó en el depósito de almacenamiento que creaste antes, y se están creando las instancias de Compute Engine. Cloud Dataflow dividirá el archivo de entrada, de manera que varias máquinas puedan procesar los datos en paralelo.

Nota: Cuando veas el mensaje "JOB_STATE_DONE", podrás cerrar Cloud Shell.

## Supervisa tu trabajo

Comprueba el progreso de tu canalización en la página de Cloud Dataflow.

### Ve a la página de IU de Cloud Dataflow Monitoring

Navega a la página de IU de Cloud Dataflow Monitoring si todavía no lo hiciste.

Abre el [menú][spotlight-console-menu] ubicado en el lado izquierdo de la consola.

Luego, selecciona la sección **Dataflow**.

<walkthrough-menu-navigation sectionid="DATAFLOW_SECTION"></walkthrough-menu-navigation>

### Selecciona tu trabajo

Haz clic en el trabajo para ver los detalles.

### Explora detalles y métricas de canalización

Explora la canalización ubicada a la izquierda y la información del trabajo a la derecha. Haz clic en [Registros][spotlight-job-logs] para ver el estado del trabajo en detalle. Puedes hacer clic en un paso de la canalización para ver las métricas.

Cuando el trabajo finalice, verás un cambio en su estado, y las instancias de Compute Engine que usó se detendrán automáticamente.

## Revisa el resultado

Ahora que se ejecutó el trabajo, puedes explorar los archivos de salida en Cloud Storage.

### Ve a la página de Cloud Storage

Abre el [menú][spotlight-console-menu] ubicado en el lado izquierdo de la consola.

Luego, selecciona la sección **Compute Engine**.

<walkthrough-menu-navigation sectionid="STORAGE_SECTION"></walkthrough-menu-navigation>

### Ve al depósito de almacenamiento

Selecciona el depósito que creaste antes desde la lista de depósitos. Si usaste el nombre sugerido, aparecerá como `{{project-id}}`.

El depósito contiene una carpeta de etapa de pruebas y carpetas de salida. Dataflow guarda los archivos de salida en fragmentos, por lo que el depósito contendrá varios archivos de salida.

## Limpieza

Para evitar cobros por el uso de Cloud Storage, borra el depósito que creaste.

### Vuelve al navegador de depósitos

Haz clic en el vínculo [Depósitos][spotlight-buckets-link].

### Selecciona el depósito

Marca la casilla que aparece junto al depósito que creaste.

### Borra el depósito

Haz clic en [Borrar][spotlight-delete-bucket] y confirma la eliminación.

## Conclusión

<walkthrough-conclusion-trophy></walkthrough-conclusion-trophy>

A continuación, puedes hacer lo siguiente:

  *  [Obtener más información sobre el ejemplo de Conteo de palabras][wordcount]
  *  [Descubrir más sobre el modelo de programación de Cloud Dataflow][df-pipelines]
  *  [Explorar el SDK de Apache Beam en GitHub][beam-sdk]

Configura tu entorno local:

  *  [Usa Java y Eclipse para ejecutar Dataflow.][df-eclipse]
  *  [Usa Java y Maven para ejecutar Dataflow.][df-maven]

[beam-sdk]: https://github.com/apache/beam/tree/master/sdks/python
[dataflow-python-tutorial]: https://cloud.google.com/dataflow/docs/quickstarts/quickstart-python
[df-eclipse]: https://cloud.google.com/dataflow/docs/quickstarts/quickstart-java-eclipse
[df-maven]: https://cloud.google.com/dataflow/docs/quickstarts/quickstart-java-maven
[df-pipelines]: https://cloud.google.com/dataflow/model/programming-model-beam
[gsutil-docs]: https://cloud.google.com/storage/docs/gsutil
[spotlight-buckets-link]: walkthrough://spotlight-pointer?cssSelector=.p6n-cloudstorage-path-link
[spotlight-console-menu]: walkthrough://spotlight-pointer?spotlightId=console-nav-menu
[spotlight-delete-bucket]: walkthrough://spotlight-pointer?cssSelector=#p6n-cloudstorage-delete-buckets
[spotlight-job-logs]: walkthrough://spotlight-pointer?cssSelector=#p6n-dax-job-logs-toggle
[spotlight-open-devshell]: walkthrough://spotlight-pointer?spotlightId=devshell-activate-button
[wordcount]: https://beam.apache.org/get-started/wordcount-example/
