# Jenkins logs

JENKINS_URL/log

Podemos encontrar logs relacionados con el controlador y plugins, debemos configurar el logger para ver eventos de plugins, por defecto existe All Jenkins Logs donde veremos logs generales del controlador.

# Log Recorders

Para ver logs de algún plugin, debemos agregar un Log Recorders, para esto agregamos y buscamos la clase a logear.

En este ejemplo, comenzamos a escribir docker y se mostrará una lista de logs que podemos ver.

![logs](/images/docker-logger.png)

Viendo logs!

![docker-logs](/images/docker-plugin-logs.png)

Es recomendable mirar estos logs cuando tenemos algún problema en nuestros pipelines que no podemos detectar desde los logs del job.
