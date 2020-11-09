#			Crear una Máquina Virtual desde CLI en Azure
Recuerda que usar el mismo grupo de recursos es indispensable para conectar 
nuestras máquinas virtuales a un balanceador de cargas. Si por error creaste 
dos grupos diferentes, lo mejor es que elimines la nueva máquina virtual y 
vuelvas a intentarlo.

Para ver todas las opciones de sistemas operativos disponibles usamos el 
comando 
```b
az vm image list.
```
Create a Resource group

```b
az group create --name grp_ubuntusrv --location eastus
```

## Create a Virtual Machine
Se debe de utilizar el siguiente comando, claro esta es un ejemplo.
```b
az vm create -n ubuntu01 -g grp_ubuntusrv --image UbuntuLTS --size Standard_B1ls --authentication-type password --admin-username userName --location eastus --generate-ssh-keys --output json --verbose 
az vm create -n ubuntu02 -g grp_ubuntusrv --image UbuntuLTS --size Standard_B1ls --authentication-type password --admin-username userName --location eastus --generate-ssh-keys --output json --verbose 
az vm create -n ubuntu03 -g grp_ubuntusrv --image UbuntuLTS --size Standard_B1ls --authentication-type password --admin-username userName --location eastus --generate-ssh-keys --output json --verbose 
```
SSH Config
```b
ssh-copy-id -i ubuntuo01.pub userName@127.0.0.1
```
No olvides usar contraseñas diferentes para cada una de tus máquinas virtuales.

get VM Information with quieres
```b
az vm show --name ubuntu01 --resource-group grp_ubuntusrv
```

Ver documentación
- https://docs.microsoft.com/en-us/azure/virtual-machines/windows/overview
- https://github.com/aminespinoza/ContenidoIaaS/tree/master/AltaDisponibilidad


