# Load Balancer en Azure
El equilibrio de carga hace referencia a la distribución uniforme de la carga (el tráfico de red entrante) en un grupo de recursos o servidores de back-end.

Azure Load Balancer opera en la capa cuatro del modelo de interconexión de sistema abierto (OSI). Es el único punto de contacto de los clientes. Load Balancer distribuye flujos de entrada que llegan al front-end del equilibrador de carga a las instancias del grupo de servidores back-end. Estos flujos están de acuerdo con las reglas de equilibrio de carga y los sondeos de estado configurados. Las instancias del grupo de back-end pueden ser instancias de Azure Virtual Machines o de un conjunto de escalado de máquinas virtuales.

- https://docs.microsoft.com/es-es/azure/load-balancer/load-balancer-overview

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
## Crear un conjunto de disponibilidad
### Paso 9: Crear 3 interfaces de red, una para cada máquina virtual.
**Linux Bash**
```b
for i in 'seq 1 3'; do az network nic create  -g grLoadBalancerTest --name myNic$i --vnet-name myVnet --subnet mySubnet --network-security-group myNetworkSecurityGroup -- lb-name myLoadBalancer --lb-address-pools myBackEndPool done
```
**Powershell**
```b
foreach ($i in 1..3)
{ az network nic create  -g grLoadBalancerTest --name myNic$i --vnet-name myVnet --subnet mySubnet --network-security-group myNetworkSecurityGroup --lb-name myLoadBalancer --lb-address-pools myBackEndPool }
```
### Paso 10: Crear un conjunto de disponibilidad.
```b
az vm availability-set create -g grLoadBalancerTest --name myAvailabilitySet
```
## Archivos de configuración inicial para máquinas virtuales
### Paso 11: Crear un archivo de instalación y configuración de arranque para las máquinas virtuales.
El archivo de instalación es para que una vez creadas las máquinas virtuales automáticamente se instalen todos los paquetes necesarios para iniciar o trabajar con nuestros desarrollos por ejemplo Instalar un apache, node, etc.  *OPCIONAL*
#### Example 
Guardar en donde se ejecute los comandos, y el nombre por ejemplo puede ser **incializar_nginx.txt** este nombre se debe de colocar dentro de los paramotros de la creacion de la maquina virtual. 
```b
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

### Paso 12: Crear 3 máquinas virtuales.
**Linux bash**
```b
for i `seq 1 3`; do
az vm create -g grLoadBalancerTest --name myVM$i --availability-set myAvailabilitySet --nics myNic$i --image UbuntuLTS --admin-username azureuser --generate-ssh-key --custom-data incializar_nginx.txt --no-wait
done
```
**PowerShell**
```b
foreach ($i in 1..3)
{ az vm create -g grLoadBalancerTest --name myVM$i --availability-set myAvailabilitySet --nics myNic$i --image UbuntuLTS --size Standard_B1ls --admin-username azadminuser --generate-ssh-key --no-wait }
```

## Verificación de todo mi entorno de trabajo
### Paso 13: Obtener la IP del balanceador de cargas. 
Puedes acceder a ella desde el portal de Azure o la línea de comandos.
```b
az network public-ip show -g grLoadBalancerTest --name ipPublic --query [ipAddress] --output tsv
```
### Paso 14: Usar la IP del balanceador de cargas para probar que la arquitectura de nuestra aplicación funciona correctamente (Ctrl + R).

## Documentación 
- https://docs.microsoft.com/es-es/azure/load-balancer/load-balancer-overview
- https://docs.microsoft.com/es-es/azure/load-balancer/load-balancer-standard-availability-zones
- **az group create**: https://docs.microsoft.com/en-us/cli/azure/group?view=azure-cli-latest#az_group_create
- **ForEach in Powershell**: https://ss64.com/ps/foreach.html



