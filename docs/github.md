# Jenkins y Github.

Para integrar Github con Jenkins lo recomendable es utilizar una [Github APP](https://docs.github.com/es/developers/apps/building-github-apps/creating-a-github-app) en conjunto al plugin [Github Plugin](https://plugins.jenkins.io/github)

Una vez configurado podemos agregar a nuestros proyectos github como origen:

![github-source](/images/github-branch-source.png)

## Webhook

Para configurar webhooks entre github y jenkins debemos habilitar GitHub hook trigger for GITScm polling:

![github-build-trigger](/images/github-build-trigger.png)

Por defecto, el plugin de github recibe webhooks en JENKINS_URL/github-webhook/ y si el repositorio que envía el webhook coincide con el repositorioa configurado en Jenkins se lanzará un build nuevo.

![github-webhook](/images/github-webhook.png)

