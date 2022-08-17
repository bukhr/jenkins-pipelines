pipeline{
    agent { label 'small'}
    environment {
        AWS_ACCOUNT="770092832210"
        ECR="770092832210.dkr.ecr.sa-east-1.amazonaws.com/sre-jenkins-training"
    }
    stages{
        stage("build"){
            steps{
                sh """
                docker build -t $ECR:dievops app
                aws ecr get-login-password --region sa-east-1 | docker login --username AWS --password-stdin 770092832210.dkr.ecr.sa-east-1.amazonaws.com
                docker push $ECR:dievops
                """
            }
        }
        stage("test"){
            agent {
                docker{
                    image "$ECR:dievops"
                    reuseNode "true"
                }
            }
            steps{
                catchError(buildResult: 'SUCCESS', stageResult: 'UNSTABLE') {
                    sh """
                    cd /app
                    pylint hello.py
                    """
                }
            }
        }
        stage("deploy"){
            options {
                timeout(time: 30, unit: 'SECONDS')
            }
            environment{
                RANDOM_PORT=sh(script: 'shuf -i 35000-36000 -n 1',returnStdout: true).trim()
            }
            steps {
            sh """
            container=\$(docker run -d -p $RANDOM_PORT:35000 $ECR:dievops python hello.py)
            docker ps -a
            curl localhost:$RANDOM_PORT || docker kill \$container
            sleep 10
            docker kill \$container
            """
            }
        }
    }
}