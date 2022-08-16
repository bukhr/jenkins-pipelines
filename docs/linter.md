# Linter Jenkinsfile.

Pasar un linter a nuestro Jenkinsfile se puede realizar tanto en un jenkins local como remoto.

## Utilizando API Token.

Para obtener un token de acceso para utilizar la api de jenkins debemos ir a nuestro usuario y generamos un token:

![user-settings](/images/user-settings.png)

![user-settings](/images/api-token.png)

### API

curl --user user:api-token -X POST -F "jenkinsfile=<Jenkinsfile" https://JENKINS_URL/pipeline-model-converter/validate

Ejemplo:

curl --user dmunoz@buk.cl: 119da51cd678b61f9acdeacd038097a241 -X POST -F "jenkinsfile=<Jenkinsfile" https://jenkins.infra.buk.cl/pipeline-model-converter/validate

## Vscode extension 