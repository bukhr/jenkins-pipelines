# Jenkinsfile

Podemos definir algunas secciones del Jenkinsfile:

![jenkinsfile](/images/descripcion-pipeline.png)

```
//comentario

/*
comentarios
Hola soy un segundo comentario
*/

// En esta sección podemos definir variables groovy, importar shared libraries, etc.

def alguna_variable = "un_valor"

@Library('name@branch') // podemos importar una shared library aquí.

pipeline {

    agent { label 'some_label' }
    environment{}
    options{}
    triggers{}
    parameters{}


    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
            post {
                always {
                    echo "always"
                }
            }
        }
        stage('Test') {
            when {}
            options{}
            steps {
                echo 'Testing..'
                script {}
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
        post {
            always {
                echo "always"
            }
        }
}
```