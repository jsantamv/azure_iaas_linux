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

### Paso 3: Crear un balanceador de cargas
```b
az network lb create -g grLoadBalancerTest -n myLoadBalancer --frontend-ip-name myFrontEndPool --backend-pool-name myBackEndPool --public-ip-address ipPublic
```
### Paso 4: Configurar un seguimiento de desempeño del balanceador de cargas. 
Agregamos una regla de seguridad para monitorear todo el comportamiento de nuestro balanceador.
```b
az network lb probe create -g grLoadBalancerTest --lb-name myLoadBalancer --name myHealthProbe --protocol tcp --port 80
```
### Paso 5: Establecer una regla de red en el balanceador de cargas.
```b
az network lb rule create -g grLoadBalancerTest --lb-name myLoadBalancer -n myLoadBalancerRule --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name myFrontEndPool --backend-pool myBackEndPool --probe-name myHealthProbe
```

### Paso 6: Crear una red virtual.
```b
az network vnet create -g grLoadBalancerTest -n myVnet --subnet-name mySubnet
```

### Paso 7: Crear un grupo de seguridad de red (NSG).
```b
az network nsg create -g grLoadBalancerTest -n myNetworkSecurityGroup
```
### Paso 8: Establecer una regla de entrada en el grupo de seguridad de red.
```b
az network nsg rule create -g grLoadBalancerTest --nsg-name myNetworkSecurityGroup --name myNetworkSecurityGroupRule --priority 1001 --protocol tcp --destination-port-range 80
```



## Documentación 
- https://docs.microsoft.com/es-es/azure/load-balancer/load-balancer-overview
- https://docs.microsoft.com/es-es/azure/load-balancer/load-balancer-standard-availability-zones
- **az group create**: https://docs.microsoft.com/en-us/cli/azure/group?view=azure-cli-latest#az_group_create



