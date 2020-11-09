# Load Balancer en Azure

## Pasos a seguir para alta disponibilidad en aplicaciones 

1. Crear un grupo de recursos
2. Crear una IP pública
3. Crear un balanceador de carga (Load Balancer)
4. Configurar un seguimiento de desempeño del Load Balancer
5. Establecer una regla de red en el Load Balancer
6. Crear una red virtual
7. Crear un grupo de seguridad de red (NSG)
8. Establecer una regla de entrada en el grupo de seguridad de red
9. Crear tres interfaces de red (una para cada máquina virtual en esta caso son 3 VM)}
10. Crear un conjunto de disponibilidad
11. Crear un archivo de instalacion y configuracion de arranque para las máquinas virtuales
12. Crear tres máquinas virtuales
13. Obtener la Ip del Load Balancer
14. Probar el balance de cargas 


## Aplicacion de los pasos
### Paso 1: Crear un grupo de recursos

ejemplo de creacion un grupo de recursos
```b
az group create --location
                --name
                [--managed-by]
                [--subscription]
                [--tags]
```
Comando aplicado
```b
az group create --name grLoadBalancerTest --location eastus
```

### Paso 2: Crear una IP Pública

```b
az network public-ip create -g grLoadBalancerTest --name ipPublic
```


## Documentación 
- https://docs.microsoft.com/es-es/azure/load-balancer/load-balancer-standard-availability-zones
- **az group create**: https://docs.microsoft.com/en-us/cli/azure/group?view=azure-cli-latest#az_group_create


