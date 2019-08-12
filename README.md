# docker-swarm-intro

## Inicializar el modo SWARM
`docker swarm init`

## Crear un servicio
`docker service create --name {service_name} {image_name} {command}`

Ejemplo: `docker service create --name pinger alpine ping www.google.com`

## Inspeccionar un servicio
`docker service ps {service_name}`

## Eliminar un servicio
`docker service rm {service_id}`

## Escalar un servicio en el manager y los workers
`docker service scale {service_name}={replicas}`
	Ejemplo: `docker service scale pinger=5`

## Ver logs de un servicio
`docker service logs -f {service_name}`

## Actualizar tareas de un servicio

### Modificar los argumentos de un servicio
`docker service update --args "{command}" {service_name}`

Ejemplo: `docker service update --args "ping www.facebook.com" pinger`

### Modificar replicas de un servicio
`docker service update --replicas={amount} {service_name}`

Ejemplo: `docker service update --replicas=20 pinger`

### Modificar paralelismo y forma de actualización de un servicio
`docker service update --update-parallelism {amount} --update-order {order} {service_name}`

Ejemplo: `docker service update --update-parallelism 4 --update-order start-first pinger`

### Modificar el paralelismo del rollback

`docker service update --update-rollback-parallelism 0 pinger`

### Actualizar configuraciones para deploy
`docker service update --update-failure-action {action} --update-max-failure-ratio {ratio} {service_name}`

Ejemplo: `docker service update --update-failure-action rollback --update-max-failure-ratio 0.5 pinger`

## Regresar un servicio a su estado anterior
`docker service rollback {service_name}`

## Crear un servicio que tiene configuración inicial de puertos y replicas
`docker service create -d --name app --publish 3000:3000 --replicas=6 omiguelperez/swarm-hostname`

## Crear un servicio de visualización
`docker service create -d --name viz -p 8080:8080 --constraint=node.role==manager --mount=type=bind,src=/var/run/docker.sock,dst=/var/run/docker.sock dockersamples/visualizer`

## Actualizar un servicio para ejecutar tareas como worker
`docker service update --constraint-add node.role==worker --update-parallelism=0 app`

## Cambiar disponibilidad de un nodo
`docker node update --availability {availability} {worker_name}`

### Poner nodo como no disponible 
Ejemplo: `docker node update --availability drain worker2`

### Poner nodo como disponible

`docker node update --availability active worker2`

`docker service update -d --env-add UNA_VARIABLE=de-entorno --update-parallelism=2 app`