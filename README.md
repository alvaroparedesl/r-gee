# Workshop de Imágenes Satelitales

## Instalación R

Desde la consola de R ejecutar: `install.packages('rgee')` e `install.packages('googledrive')`. Revisar las salidas por si encuentran algún error. En Windows debiera funcionar sin problemas, pero en Linux es posible que deban realizar algunos pasos extras.

### Linux (Ubuntu 20.04+)

Si los anteriores comandos no funcionaron, revisar bien los mensajes de error por algunas dependencias que pudieran faltar. Algunas de estas dependencias, se solucionan con el comando `sudo apt-get install -y libprotobuf-dev protobuf-compiler protolite`.

Si aún no funciona, puede ser debido a diferentes versiones de la librería protoc. En Ubuntu 20.04, la solución es agregar un repositorio externo con paquetes más actualizados: `sudo add-apt-repository ppa:savoury1/backports & sudo apt install libprotobuf-dev` que instalará la versión `protobuf==3.12.4`.

Luego volver a intentar instalar los paquetes correspondientes. El paquete de R "conflictivo" es `geojsonio`, específicamente la dependencia `protolite`.

Para otras versiones de Linux o más antiguas que 20.04, buscar los paquetes equivalentes.


## Instalación ambiente Python

La primera opción es usar `rgee::ee_install(py_env = "gee")` desde R que hace un intento por hacer una instalación automática de todo lo necesario en Python, usando Miniconda (si no lo tienen, intentará descargarlo). Pueden probar esa opción, pero personalmente prefiero hacerlo de manera manual.

Teniendo conda o miniconda instalados, ejecutar:

`conda create -y --name gee python=3.8 earthengine-api numpy`

Si todo funciona sin problemas, esto generará un ambiente llamado `gee` en Python. Por el momento no es necesario hacer nada más

> Nota: si necesitan eliminar o cambiar la instalación del ambiente, pueden hacerlo con `conda remove --name gee --all -y`.


## Configuración R

Con todo lo demás saneado, falta sólo configurar R para que conecte adecuadamente con el ambiente.

Primero, desde la consola de R se puede verificar que todo esté instalado correctamente con `rgee::ee_check()`. Lo más probable es que arroje errores, porque no encuentra el ambiente Python (si es que no usaron `ee_install`).

Para configurar el ambiente. En mi caso:

```R
rgee::ee_install_set_pyenv(
  py_path = "C:/Users/alvaro/anaconda3/envs/gee",
  py_env = "gee"
)
```
para Windows o:

```R
rgee::ee_install_set_pyenv(
  py_path = "/home/alvaro/anaconda3/envs/gee/bin/python3",
  py_env = "gee"
)
```
para Linux.

Volver a revisar con `rgee::ee_check()` y no debieran saltar errores.

Para configurar el usuario, debiera bastar con `rgee::ee_Initialize(user = 'alvaro', drive = TRUE)` y les pedirá habilitar las credenciales en Google (importante notar que ya deben tener acceso a Google Earth Engine). Si en algún paso pide *Please insert the desired name of your root folder*, usar users/nombre (en mi caso, users/alvaro).

Si hasta acá todo funcionó bien, están listos para comenzar.

Otras funciones que pueden ser de interés: 

  1. `ee_install_upgrade()` para actualizar la versión de earthengine-api en Python (desde R por supuesto). 
  1. `ee_clean_credentials()` para limpiar credenciales.
  1. `ee_check_python()` para ver la versión de Python.
  1. `ee_check_credentials()` para las credenciales de Google Drive y GCS.
  1. `ee_check_python_packages()` para ver los paquete de Python.


> Nota: es posible que sea necesario instalar el SDK de Google para que funcione. No debiera, pero en caso de necesitarlo dirigirse a [esta página](https://cloud.google.com/sdk/docs/install)


