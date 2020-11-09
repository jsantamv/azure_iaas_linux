

#Galería de imágenes de Máquinas Virtuales
#Antes de elegir tu máquina virtual debes preguntarte:
#
#¿Para qué las vas a utilizar?
#¿Cuánto tiempo la mantendrás encendida?
#¿Se trata de una aplicación productiva o de investigación?
#¿Cuál será el retorno de inversión de tu máquina virtual?
#Recuerda que el tamaño y costo de tu máquina virtual 
#dependen al 100% de tu respuesta a estas preguntas.

#--------------------------------------------------------------------------
#		Instalación de Azure CLI en Ubuntu Bash 20.20
#--------------------------------------------------------------------------
#Antes de crear, configurar o acceder a nuestras máquinas virtuales desde la terminal debemos instalar las herramientas de desarrollo de Azure.

Instalación
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash

Login
az login

documentacion
https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-windows?view=azure-cli-latest
