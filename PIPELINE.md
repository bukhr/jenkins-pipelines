# Cómo construir Pipelines.

Existen 2 tipos de pipelines en Jenkins:

1 Scripted

2 Declarative

## Scripted

Scripted ya no se recomienda pero está soportado por Jenkins, posee una complejidad mayor y una curva de aprendizaje mayor a Declarative.

Ejemplo:

'''
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
'''
## Declarative

Declarative es la forma recomendada en la actualidad para crear pipelines, aquí es posible realizar todo lo que puedes hacer en scripted, incluso código groovy al interior de script{ def var = "example" }

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

Si es necesario crear algún paso complejo que no es posible realizar con la sintaxis declarativa, podemos incluirlo como script, ejemplo:

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

# Node (la máquina)

Un nodo es 1 máquina, física o virtual, con x cantidad de recursos.


# Agentes (Archivo agent.jar que reporta al controller, puede ser JNLP o SSH)

Los agentes en Jenkins pueden ser del tipo Unix, Windows, Mac. Estos se agregan en Manage Jenkins > Manage nodes and clouds. Cada agente posee etiquetas, labels, que permiten diferenciarlos entre si, podemos declarar una gente principal en nuestro Jenkinsfile de la siguiente forma:

### jenkinsfile
pipeline {
    agent { label 'ubuntu-20' }
    ...
}

Esto quiere decir que el pipeline correrá por defecto en el agente con la etiqueta "ubuntu-20"

Agente distinto para cada stage:

### Jenkinsfile
pipeline {
    agent { label 'ubuntu-20' }

stages {
  stage('Windows tests'){
    agent { label 'Windows'}
  }

  stage('Unix tests'){
    agent { label 'ubuntu-20'} // Si volvemos a declara el agente, se usará un executor de ese agente.
  }

}
}

# Executor

Un executor es una división lógica de un agente, hay recomendaciones para configurar la cantidad de executores x agente, entre ellas tenemos:

- 1 executor por nodo es lo más seguro.
- 1 executor por CPU funcionará bien si la tarea es simple.
- Podemos tener más executores por CPU si conocemos la carga de trabajo.


# Controller, Agent, Executor

![Agentes](/images/agentes.png)