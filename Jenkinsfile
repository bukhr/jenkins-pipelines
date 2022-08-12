pipeline{
    agent{
        label "small"
    }
    environment{
        GIT_COMMIT="1c0bc9a87e1abafb330ef7443765a1e6ad88c7a4" //esta variable viene desde git, se encuentra declara aqui solo como ejemplo.
        DOCKER_IMAGE="ubuntu:latest"
        AWS_ACCOUNT="770092832210"
        COMMIT_SHA = "${GIT_COMMIT[0..7]}"
        RANDOM_VALUE = sh(script: 'shuf -n 1 -e 1 22 333 4444 55555 666666',returnStdout: true).trim()
        ULTRA_SECRET_TOKEN = credentials('some-secret-text')
        ALL_MY_ENVS_WHY_NOT = """${DOCKER_IMAGE}-${AWS_ACCOUNT}/${RANDOM_VALUE}---${ULTRA_SECRET_TOKEN}"""
    }
    triggers{
        cron('H 13 * * *') // H viene de hash, quiere decir que si multiples jobs se declaran con el mismo crontab no se lanzan juntos, si no que garantiza que se  lancen en ese periodo, aqui es: en algún minuto de esa hora.
    }
    options {parallelsAlwaysFailFast()
             timeout(time: 180, unit: "MINUTES")
             disableConcurrentBuilds(abortPrevious: true)}
    parameters{
            string(name: 'COLOR', defaultValue: "negro", description: 'tu color')
            string(name: 'FRUTA', defaultValue: 'manzana', description: 'alguna frutita')
            string(name: 'NOMBRE', defaultValue: 'juan', description: 'tu nombre')
            }
    stages{
        stage("A"){
            steps{
                echo "Este es el stage $STAGE_NAME"
                sh """
                echo Este es el stage $STAGE_NAME
                date
                """
            }
            post{
                always{
                    echo "Siempre muestro este mensaje"
                }
                success{
                    echo "Todo bien"
                }
                failure{
                    echo "Algo falla"
                }
            }
        }

        stage("B"){
            steps{
                echo "Este es el stage $STAGE_NAME"
                //groovy - scripted
                script {
                    def mi_lista = ["soy","un","ejemplo"]
                    mi_lista.each { valor -> println valor } 
                }
            }
        }

        stage("C-D"){
            parallel{
                stage("C"){
                    agent {
                        docker {
                            image "$DOCKER_IMAGE"
                            args '-u root'
                            reuseNode true
                        }
                    }
                    steps{
                        sh """
                        echo $ALL_MY_ENVS_WHY_NOT
                        """
                    }
                  }

                stage("D"){
                    environment {
                        NOMBRE = "condorito"
                    }
                    agent {
                        docker {
                            label 'medium'
                            image "$DOCKER_IMAGE"
                            args '-u root'
                            reuseNode false
                        }
                    }
                    steps{
                    echo "hola $NOMBRE veo que comes $FRUTA y tu color es $COLOR"
                    }
                }
            }
        }

          stage('BuildAndTest') {
            matrix {
                agent { label 'small' }
                axes {
                    axis {
                        name 'PLATFORM'
                        values 'linux', 'windows', 'mac'
                    }
                    axis {
                        name 'BROWSER'
                        values 'firefox', 'chrome', 'safari', 'edge'
                    }
                }
                stages {
                    stage('Build') {
                        steps {
                            echo "Do Build for ${PLATFORM} - ${BROWSER}"
                        }
                    }
                    stage('Test') {
                        steps {
                            echo "Do Test for ${PLATFORM} - ${BROWSER}"
                        }
                    }
                }
            }
        }

        stage("burger") {
            steps {
                build job: 'sre-jenkins-training', parameters: [string(name: 'COMIDA', value: "$STAGE_NAME")]
            }
        }

        stage("inputs"){
          input {
            message "Continuamos?"
            ok "Si"
            submitter "Administer" //User IDs and/or external group names of person or people permitted to respond to the input, separated by ','.
            parameters {
              string(name: 'PERSON', defaultValue: 'Mr SRE', description: 'Saludamos')
            }
          }
          steps{
            sh """
            echo "hola $PERSON"
            """
          }
        }

    // SSH
        stage("ssh-agent") { //lo que se encuentra en el bloque sshagent corre en una sesion ssh temporal, se crea una temp-key.pem en el agente y se utiliza para autenticar
            steps {
                catchError(buildResult: 'SUCCESS', message: 'esto falla no hay key ssh', stageResult: 'FAILURE') { //build result=resultado del pipeline. stageResult=estado del stage
                sh """
                git clone git@gitlab.com:bukhr/buk-webapp.git
                """
                }
                sshagent(['bermuditas-ssh']) {
                sh """
                git clone git@gitlab.com:bukhr/buk-webapp.git
                """
                }
                build job: 'sre-jenkins-training', parameters: [string(name: 'COMIDA', value: "$STAGE_NAME")]
            }
        }

    }

    post{
        always{
            echo "Post en always de todo el pipeline"
        }
        success{
            echo "Post si el pipeline es exitoso"
        }
        failure{
            echo "Post si el pipeline es fallido"
        }
    }
}