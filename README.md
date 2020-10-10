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


## Ejecutando el template

Desde AWS Management Console:
Desde un navegador abrir https://aws.amazon.com/. Logearse y escojer una región. Buscar CloudFormation en los menues (o utilizar la funcion de búsqueda de servicios si es necesario). Una vez en el menú de CloudFormation, click en `Create Stack`. Seleccionar la opcion de subir tu propio template y click en *next*

Dio algún erorr?, de ser asi corregirlos y volver a subir el template

En la pagina siguiente, darle un nombre al stack. El nombre puede ser como quieras que se llame, darle next-next-next sin hacer ningún otro cambio (está todo en el template)

Cuando CloudFormation comience a ejecutar, es bueno mirar la pestaña de `Events` y `Resources`. En caso de haber algún error, el stack va a mostrar que existe un error o falló un paso, en la pestaña de `Events` se puede revisar en que momento fallo y tener indicios de como arreglarlo, además CoudFormation va a hacer rollback de todo lo generado hasta ese punto y abortará la creación de más recursos del *stack*

Desde AWS CLI:
La mejor opcion es siempre utilizar CLI, para hacer lo mismo que está descrito en las lineas anteriores hacemos lo siguiente:

`aws cloudformation create-stack --stack-name MyNetwork --template-body file://MyNetwork.yml`

Donde:
- --stack-name: Es el nombre del stack, recordar que puede ser cualquier nombre
- --template-body: El archivo sobre el cual hemos estado trabajando, idealmente este debe tener la ruta absoluta del archivo

Para ver que es lo que está haciendo CloudFormation under the hood debemos utilizar el comando:

`aws cloudformation wait stack-create-complete --stack-name MyNetwork`

... Y cuando el stack haya terminado, podemos revisarlo con:

`aws cloudformation describe-stacks` 

Revisa la VPC:
Desde AWS Management Console, ve a la sección de VPC, va a existir una VPC con el nombre generado


## Modificar el template

Desde AWS Management Console:
Seleccionar el stack que queremos actualizar desde la lista y escojer la accion *"Update Stack"*. Seleccionar la opcion de subir tu propio template y darle next. Darle next hasta llegar a la seccion de crear un *change set*. *Change set* es literalmente los cambios que CloudFormation pretende aplicar a los recursos. CloudFormation puede hacer algunos cambios de manera facil, pero otros cambios pueden requerir que algunos recursos existentes sean eliminados y re-creados. Elegir la última opcion de ejecutar el *change set* y observar los cambios realizados.

Desde AWS CLI:
Para modificar nuestro template utilizamos el comando:

`aws cloudformation update-stack --stack-name MyNetwork --template-body file://MyNetwork.yml` 

Y como anteriormente, podemos utilizar el comando `wait` para monitorear el progreso:

`aws cloudformation wait stack-update-complete --stack-name MyNetwork`


## Eliminar el stack

Un concepto nativo de la nube (de cualquier proveedor) y que hay que acostumbrarse es que todos los *recursos son desechables*, por lo tanto podemos crear y borrar nuestro stack cuando sea que queramos ahora que tenemos nuestro template, para hacerlo desde AWS Management Console simplemente seleccionamos el stack y elegimos la accion *delete*.

Desde AWS CLI utilizamos el siguiente comando: `aws cloudformation delete-stack --stack-name MyNetwork`


## Documentacion de todos los recursos y servicios utilizados:

- AWS Management Console: https://aws.amazon.com/
- AWS CLI: https://aws.amazon.com/cli/
- CloudFormation: https://aws.amazon.com/cloudformation/
- VPC: https://aws.amazon.com/vpc/ - https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Subnets.html
- Internet Gateway: https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html
- NAT Gateway: https://docs.aws.amazon.com/vpc/latest/userguide/vpc-nat.html

## Guia completa creada por Ken Krugger
- Parte 1: https://www.infoq.com/articles/aws-vpc-cloudformation/
- Parte 2: https://www.infoq.com/articles/aws-vpc-cloudformation-part2/

