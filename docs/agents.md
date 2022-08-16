# Agents

Por defecto, jenkins se lanzará a si mismo como agent con 2 executors, esta configuración no es recomdable para ambientes con altas cargas de trabajo, debido a que el mismo con controller funcionará como agente, perjudicanto la performance web si las tareas son muy exigentes.

Cambiamos esto en:

![jenkins url](/images/default-executors.png)

# Node (la máquina)

Un nodo es 1 máquina, física o virtual, con x cantidad de recursos.

# Agentes

El agente puede interpretarse como el proceso agent.jar que reporta al controller logs, estado de la máquina. Este agente puede ser JNLP o SSH, se recomiendo agregar agentes utilizando SSH.

Los agentes en Jenkins pueden ser del tipo Unix, Windows, Mac. Estos se agregan en Manage Jenkins > Manage nodes and clouds. Cada agente posee etiquetas/labels, que permiten diferenciarlos entre si, podemos declarar una gente principal en nuestro Jenkinsfile de la siguiente forma:

# Executor

Un executor es una división lógica de un agente, hay recomendaciones para configurar la cantidad de executores x agente, entre ellas tenemos:

- 1 executor por nodo es lo más seguro.
- 1 executor por CPU funcionará bien si la tarea es simple.
- Podemos tener más executores por CPU si conocemos la carga de trabajo.

```
### jenkinsfile
pipeline {
    agent { label 'ubuntu-20' } //aquí se utiliza 1 executor en el agente.
    ...
}
```

Esto quiere decir que el pipeline correrá por defecto en el agente con la etiqueta "ubuntu-20" utilizando 1 executor.

Agente distinto para cada stage:

```
### Jenkinsfile
pipeline {
    agent { label 'ubuntu-20' } //aquí se utiliza 1 executor en el agente.

stages {
  stage('Windows tests'){
    agent { label 'Windows'}
  }

  stage('Unix tests'){
    agent { label 'ubuntu-20'} // Si volvemos a declara el agente, se usará un executor de ese agente.
  }
}
}
```

Si los agentes no poseen executor disponible, el job quedará en la cola, si tenemos algún plugin como AWS EC2, se lanzará una instancia nueva que corresponda a la etiqueta declarada.


# Controller, Agent, Executor

![Agentes](/images/agentes.png)

# Jenkins Cloud Agents.

Jenkins permite agregar agentes estáticos y lanzar agentes dinámicos en proveedores de servicios, conocidos como cloud. Un agente cloud puede ser un pod de kubernetes o instancias de algún proveedor cloud: AWS GCP AZURE DIGITALOCEAN etc.

## JENKINS EC2 PLUGIN

https://plugins.jenkins.io/ec2/

Este plugin permite lanzar agentes en aws utilizando parámetros y una AMI.

Vamos a JENKINS_URL/configureClouds

![add-cloud](/images/add-cloud.png)

Completamos la configuración relacionada con el plugin

![ec2-settings](/images/ec2-settings.png)

Algunos parámetros:

- AMI: id de ami a utilizar.
- Labels: nombre del agente, este es el nombre que utilizamos en agent { label 'small' }
- Remote user: ubuntu, ec2-user, admin, etc. Depende de la ami.
- Number of Executors: Cantidad de executors, debemos conocer la carga de trabajo para configurar este valor, se recomienda comenzar con 1.
- Connection Strategy: IP pública o IP privada.
- Block device mapping: /dev/sda1=:500 mapeamos la cantidad de disco y el punto de montaje deseado(revisar AMI)
- Init-Script: comandos que correrán en el agente al bootear(apt update && apt install docker.io ...)
- Subnets: subnets id separados por coma.
- Availability Zone: Ids separadas por coma.
- Instance Type: Seleccionamos tipo de instancia.
- Tags: lleva:valor para identificar en EC2 nuestros agentes.
- Security group names: nombre del sg que se asociará al agente.
- Idle termination time: Si el agente no tiene pipelines asignados, se esperará este tiempo en minutos para su borrado/apagado.
