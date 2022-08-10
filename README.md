# Jenkins Pipeline Training

Este proyecto busca implementar un pipeline sencillo para entender conceptos y funcionamiento de jenkins declarative pipeline.

Para profundizar aún más es recomendable leer: https://www.jenkins.io/user-handbook.pdf

# Instalación de Jenkins controller con docker.

Iniciamos un controller con:

docker run -it -p 8080:8080 --rm jenkins/jenkins

Al iniciar jenkins por primera vez, nos entregará por consola la contraseña de administador para iniciar el asistente de configuración.

![Unlock jenkins](/images/unlock-jenkins.png "Copy password from terminal")