# Procedimiento para implementar una aplicacion en AWS Elastic Beanstalk.
Esta es la documentación del procedimiento para implementar una aplicacion en AWS Elastic Beanstalk con EB CLI desde Ubuntu.

## Paso 1:
Lo primero que se tiene que hacer es instalar **Python**, **pip** y **EB CLI**.

### Comprobar si se tiene Python instalado.
```bash
python3 --version
```
En caso de no tenerlo instalado, procedemos a instalar la version de Python que deseamos, preferentemente la ultima version (actualmente es la 3.8).
```bash
sudo apt-get install python3.8
```
Ahora verificamos que la instalación sea la correcta mediante el siguiente comando.
```bash
python3 --version
```
### Instalar pip.
Procedemos a descargar el script de instalacion para pip.
```bash
curl -O https://bootstrap.pypa.io/get-pip.py
```
Ahora ejecutamos el script con Python.
```bash
python3 get-pip.py --user
```
Ahora se añade la ruta del ejecutable a la variable *PATH*, normalmente en Ubuntu es *~/.local/bin*.
Ahora buscamos el script de perfil en la shell en la carpeta de usuario, si no se sabe cual es la shell que se tiene, ejecuta el siguiente comando.
```bash
echo $SHELL
```
Despues se necesita saber donde esta el script del profile, para eso ejecute:
```bash
ls -a ~
```
Normalmente en bash es *.bash_profile*, *.profile* o *.bahs_login*
Ahora añadimos el comando de exportacion del script al perfil, un ejemplo si la ruta ejecutable es *~/.local/bin*:
```bash
export PATH=~/.local/bin:$PATH
```
Despues cargamos el script del perfil, en este caso es *.profile*
```bash
source ~/.profile
```
Ahora comprobamos la vesion de pip.
```bash
pip --version
```
### Instalar EB CLI.
Utilizamos pip para instalar EB CLI.
```bash
pip install awsebcli --upgrade --user
```
Finalmente comprobamos la version de EB CLI.
```bash
eb --version
```
Para actualizar a la version mas reciente, ejecutar el siguiente comando.
```bash
pip install awsebcli --upgrade --user
```
## Paso 2:
Ahora procedemos a la inicialización del Git en la carpeta de nuestro proyecto.
```bash
git init
```
Y añadimos un archivo llamado .gitignore y añadimos lo siguiente:
```
node_modules/
.gitignore
.elasticbeanstalk/
```
Tenemos que asegurarnos que en nuestro archivo package.json tengamos identificada la version de node que se esta utilizando, con lo siguiente:
```
{
    ...},
    "engines": {
        "node": "12.18.4 "
    }
}
```
Y que tambien tengamos instalado el aws-sdk.
```bash
npm install aws-sdk
```
## Paso 3:
Creamos un entorno en Elastic Beanstalk, en este caso la plataforma es *node.js* y la region es *us-west-1*.
Lo primero es crear un repositorio con el comando *eb init*.
```bash
eb init --platform node.js --region us-west-1
```
Despues creamos el entorno con una aplicacion de ejemplo con el comando *eb create*, el nombre del entorno en este caso es node-express-env.
```bash
eb create --sample node-express-env
```
Para abrir el entorno en el navegador solo ejecutamos *eb open*.
```bash
eb open
```
## Paso 4:
Para implementar o actualizar la aplicación primero debemos almacenar temporalmente los archivos:
```
git add .
```
```
git commit -m "Commit"
```
Y finalmente implementamos los cambios. 
```
eb deploy
```
