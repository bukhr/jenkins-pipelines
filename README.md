# Jenkins Pipeline Training

Este proyecto busca implementar un pipeline para entender conceptos y funcionamiento de jenkins declarative pipeline.

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

Se recomienda instalar solo plugins necesarios tu jenkins controller productivo, sin embargo muchos plugins poseen dependencias que hacen necesarios instalar componentes adicionales.

Cada plugin y su documentación se puede encontrar aquí: https://plugins.jenkins.io/

## JENKINS URL

Este parámetro es uno de los más importantes de nuestro servidor, esta url define al controlador mismo y su propia dirección, para la instalación local será http://localhost:8080, agentes e integraciones utilizan este endpoint para interactuar con el controlador, si necesitas publicar el servidor debemos cambiar este parámetro desde manage jenkins, por ejemplo: https://jenkins.midominio.com (tras un lb)

![jenkins url](/images/jenkins-url.png)