# Taller Jenkins.

Vamos a construir algo!

Nuestro equipo necesita realizar un pipeline en jenkins con los siguientes requerimientos:

- Source: Github
- Stages: Build, Test, Deploy.
- Build: construir imagen docker y pushear a un ecr
- Test: 1 linter con Pylint que permita fallar sin declarar el pipeline como fallido.
- Deploy: correr la app con docker y validar con curl.

Pasos:

1. Crear en Jenkins un proyecto del tipo pipeline con nombre: sre-jenkins-<nombre>
2. Activar GitHub hook trigger for GITScm polling
3. En la sección pipeline seleccionar "Pipeline from SCM"
4. En Branches to build ingresar "**" o dejar en blanco.
4. Repository URL: https://github.com/org/jenkins-training, donde org es tu organización o nombre.
5. Escribir el Jenkinsfile.
6. Pasar el Linter al Jenkinsfile.
7. Iterar
