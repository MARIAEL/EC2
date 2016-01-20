
# Instalar yeoman

Seguir los pasos de [yeoman](http://yeoman.io/codelab/setup.html) o bien los pasos del cuadro gris a continuación

```bash
# PASO 1
$ descargar e instalar MEAN -> # https://bitnami.com/redirect/to/86975/bitnami-meanstack-3.2.1-0-windows-installer.exe

# PASO 2 Instalar yo, grunt y bower ( desde Inicio en windows ir a bitnami y abrir "Use Bitnami MEAN stack" )
$ npm install --global yo bower grunt-cli

# PASO 3 instalar el generador de angular
$ npm install --global generator-angular generator-karma
```
# Usar  yeoman para crear web


```bash
# 
$ mkdir micarpeta
$ cd micarpeta
$ yo
```
Si no se ejecuta Yeoman volver a instalar estos elementos por separado:  

```bash
# 
$ npm install --global yo  
$ npm install --global bower  
$ npm install --global grunt-cli  
```
Una vez que finalice la instalación poner en el terminal:  

```bash
# 
$ mkdir micarpeta
$ cd micarpeta
$ yo
```
Durante la ejecución de Yeoman nos hará unas preguntas a las que hay que responder:  


What would you like to do?  
Angular  
Would you like to use Gulp instead of Grunt?  
No  
Would you like to use Sass?  
No  
Would you like to include Bootstrap?  
Yes  
Which modules would you like to include?  
Press ENTER con los que haya preseleccionados.  

Una vez haya creado Yeoman hay que arrancar el servidor, para ello pondremos en el terminal  
```bash
# 
$ grunt serve

```

Ya podemos empezar a trabajar con nuestro archivo web.  
