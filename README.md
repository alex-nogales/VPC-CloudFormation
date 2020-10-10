# Cloudformation template para creacion automatizada de VPCs

## Infrastrucutre as Code (IaaC)
CloudFormation nos permite crear *"stacks"* de *"recursos"* en un solo paso. Los recursos son todas las cosas que creamos (Instancias EC2, VPCs, subnets, etc), un grupo de recursos es llamado *stack*. La idea es crear un template para generar una red (network) exactamente como queramos en un solo paso.
Es preferible utilizar IaaC en vez de Consolas para este tipo de deploy debido a que IaaC es rápido, repetible y mucho más consistente que realizar los cambios de manera manual en la Consola o CLI.
Adicionalmente, al utilizar IaaC podemos utilizar herramientas de source control (como git) para utilizar nuestro codigo cuando se nos antoje!

A continuación los tres puntos más importantes de por qué utilizar:

- Updateable: Podemos moficiar el stack de redes haciendo cambios en nuestro template de CloudFormation sin necesidad de reescribir todo desde cero. Además CloudFormation es lo suficientemente inteligente para cambiar la configuración del stack para igualar el template
- Reusable: Se puede utilizar el mismo template para crear multiples redes en regiuones distintas, con propósitos distintos... hasta en cuentas distintas!
- Drift Detection: CloudFormation tiene la capacidad de avisarnos cuando un recurso está `drifted` (distinto) de la configuración original. Esto se da cuando por ejemplo un administrador cambia manualmente algún recurso que pertenezca al stack de CloudFormation
- Disposable: podemos eliminar el stack y por consiguiente todos los recursos con un solo click (o linea de comando :P)
