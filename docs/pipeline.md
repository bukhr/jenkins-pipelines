# Pipeline

Aquí se describe algunos conceptos sobre pipelines.

# WORKSPACE

Antes de comenzar veremos una de las cosas más importantes, el $WORKSPACE.

WORKSPACE es nuestro directorio de trabajo, aquí se clona el repositorio, se archivan artefactos otras operaciones.

Cuando ocupamos un agent {docker {..} }, jenkins monta en el container la misma ruta worskpace, por ejemplo:

Directamente en el agente:

WORKSPACE=/home/ubuntu/workspace/main

En el container con docker:

docker run -t -v $WORSPACE:/home/ubuntu/workspace/main (-v $WORSPACE:$WORSPACE)

Esto quiere decir que nuestro directorio de trabajo se encuentra montado al interior del contenedor.

## Variables de entorno

Al igual que con WORKSPACE, jenkins agrega todas las variables de entorno al container.

## Log de una ejecución en docker.

```
$ docker run -t -d -u 1000:1000 -w /home/ubuntu/workspace/buk-webapp-github_master -v /home/ubuntu/workspace/buk-webapp-github_master:/home/ubuntu/workspace/buk-webapp-github_master:rw,z -v /home/ubuntu/workspace/buk-webapp-github_master@tmp:/home/ubuntu/workspace/buk-webapp-github_master@tmp:rw,z -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** -e ******** 770092832210.dkr.ecr.sa-east-1.amazonaws.com/buk/buk-webapp-sa:test-2d7043e3
```

# Declarative/Scripted

Existen 2 tipos de pipelines en Jenkins:

1 Scripted

2 Declarative

## Scripted

Scripted ya no se recomienda pero está soportado por Jenkins, posee una complejidad mayor a Declarative.

Ejemplo:

```
#### Jenkinsfile
node {
  stage('Build') {
  sh 'make'
  }
  stage('Test') {
  sh 'make check'
  junit 'reports/**/*.xml'
  }
  stage('Deploy') {
  sh 'make publish'
  }
}
```
## Declarative

Declarative es la forma recomendada en la actualidad para crear pipelines, aquí es posible realizar todo lo que puedes hacer en scripted, incluso código groovy al interior de script{ def var = "example" }

```
### Jenkinsfile
// esto es un comentario
pipeline {
  agent any
    stages {
      stage('Build') {
        steps {
          sh 'make'
        }
      }
      stage('Test'){
        steps {
          sh 'make check'
          junit 'reports/**/*.xml'
        }
      }
      stage('Deploy'){
        steps {
        sh 'make publish'
        }
      }
    }
}
```

Si es necesario crear algún paso complejo que no es posible realizar con la sintaxis declarativa, podemos incluirlo como script, ejemplo:

```
stage("docker sidecar"){
    steps{
        script {
        docker.image('mysql:5').withRun('-e "MYSQL_ROOT_PASSWORD=my-secret-pw"') { c ->
          docker.image('mysql:5').inside("--link ${c.id}:db") {
            /* Wait until mysql service is up */
            sh 'while ! mysqladmin ping -hdb --silent; do sleep 1; done'
        }
        docker.image('centos:7').inside("--link ${c.id}:db") {
            /*
             * Run some tests which require MySQL, and assume that it is
             * available on the host name `db`
             */
            sh 'make check'
        }
    }
        }
    }
}
```

## Variables.

Las variables pueden ser llamadas como $VARIABLE si se declaran como variable de entorno.

Una variable declarada en un script debe incluir un caracter de escape:

environment {
  VARIABLE_Z="Z"
}
sh """
export VARIABLE_A="A"
echo \$VARIABLE_A //esta es la variable declarada en la shell.
echo $VARIABLE_Z //esta es la variable de entorno declarada en el jenkinsfile.
"""