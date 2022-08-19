# Jenkins Pipeline Training

Este proyecto busca implementar un proyecto sencillo para entender conceptos y funcionamiento de jenkins declarative pipeline.

Para profundizar aún más es recomendable leer: https://www.jenkins.io/user-handbook.pdf

# Instalación de Jenkins controller con docker.

## Iniciamos localmente un controller

docker run -it -p 8080:8080 --rm jenkins/jenkins

## Al iniciar jenkins por primera vez, nos entregará por consola la contraseña de administador para iniciar el asistente de configuración.

![Unlock jenkins](/images/unlock-jenkins.png "Copy password from terminal")

## Para agilizar esta guía seleccionamos "select plugins to install"

![select plugins](/images/select-plugins-to-install.png)

## Seleccionamos ALL para hacernos una idea la cantidad de integraciones/plugins que tenemos disponibles.

![Install plugins](/images/install-all-plugins.png)

## Sobre Plugins.

Se recomienda instalar solo plugins necesarios en jenkins controller productivos, sin embargo muchos plugins poseen dependencias que hacen necesarios instalar componentes adicionales.

Cada plugin y su documentación se puede encontrar aquí: https://plugins.jenkins.io/

## JENKINS URL

Este parámetro es uno de los más importantes de nuestro servidor, esta url define al controlador mismo y su propia dirección, para la instalación local será http://localhost:8080, agentes e integraciones utilizan este endpoint para interactuar con el controlador, si necesitas publicar el servidor debemos cambiar este parámetro desde manage jenkins, por ejemplo: https://jenkins.midominio.com (tras un lb)

![jenkins url](/images/jenkins-url.png)

# Pipeline.

Jenkins es un servidor de automatización y su principal componente son pipelines, las ventajas de utilizar pipelines son:

- Código: los pipelines se implementan en el código y, por lo general, se encuentran como archivo Jenkinsfile en el código fuente, lo que proporciona a equipos la capacidad de editar, revisar e iterar.

- Duradero: los pipelines pueden sobrevivir tanto a los reinicios planificados como a los no planificados del servidor Jenkins.

- Pausable: los pipelines pueden detenerse opcionalmente y esperar la aprobación o el aporte humano antes de continuando la ejecución.

- Versátil: los pipelines admiten requisitos complejos de entrega continua del mundo real, incluidos hacer bucles y realizar trabajos en paralelo.

- Extensible: el plugin Pipeline admite extensiones personalizadas para su DSL: domain specific language y múltiples opciones de integración con otros plugins.

## Paths en Jenkins controller que son de gran ayuda.

Debido a la gran cantidad de opciones y pasos que podemos hacer en jenkins existen algunos paths en el controller que son de gran ayuda:

### http://localhost:8080/pipeline-syntax/

Aquí podemos encontrar Snippet Generator, que son fragmentos de código que podemos utilizar en la sección "steps"

### http://localhost:8080/directive-generator/

Aquí podemos generar las secciones al interior de la sección stage('build'){ directive {} }

### http://localhost:8080/env-vars.html/

Variables de entornos por defecto en cada pipeline.

# Lectura recomendada.

Debemos conocer:

https://www.jenkins.io/doc/book/pipeline/syntax/

https://www.jenkins.io/user-handbook.pdf

# Siguientes pasos.

## Agent

[Agents](/docs/agents.md)

## Pipeline

[Pipeline](/docs/pipeline.md)

## Jenkinsfile

[Jenkinsfile](/docs/jenkinsfile.md)

## Linter

[Linter](/docs/linter.md)

## Logs

[Logs](/docs/logs.md)

## Github

[Github](/docs/github.md)

## Taller

[Tarea](/docs/taller.md)
