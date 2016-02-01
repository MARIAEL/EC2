
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
```#PASO 4 comprobar que tenemos todo lo necesario instalado
$ node --version
$ npm --version       # si no funciona  -> npm install --global npm@latest
$ git --version        
$ yo --version        # si no funciona  -> npm install --global yo 
$ bower --version     # si no funciona  -> npm install --global bower 
$ grunt --version     # si no funciona  -> npm install --global grunt-cli```

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

Ya podemos empezar a trabajar en nuestro proyecto web.  

Cada vez que tengamos que iniciar una nueva sesión de trabajo, hay que ir al terminal y colocarse en la carpeta del proyecto (micarpeta) y desde allí arrancar el servidor.  

```bash
# 
$ grunt serve

```

#PASOS PARA HACER UNA NUEVA WEB  

1.- Abrir el terminal de Bitnami MEAN Strack  

$ ls  
$ cd apps  
$ mkdir zapatos  
$ cd zapatos  
$ yo angular (responder a las preguntas que nos haga según las indicaciones de más arriba)  


Arrancar el servidor  

$ grunt serve  

Si vemos que no ha cogido el Bootstrap parar el servidor (Ctrl+C) y poner en el terminal:  
$ bower install bootstrap --save  
Si aún así no lo instala poner la siguiente linea antes de la hoja de estilos de la cabecera del index:  

    <link rel="stylesheet" href="bower_components/bootstrap//dist/css/bootstrap.css"/>

2.- Creamos las páginas que queremos que tenga nuestra web  

$yo angular:route contacto (pe.)  
$yo angular:route catalogo (etc..)  

3.- Yeoman nos habrá creado la infraestructura de js y html (que nosotros tendremos que retocar)  

4.- Tendremos que hacer los siguientes cambios:  
  - En index.html crear las li que sean necesarias  

```bash
            <ul class="nav navbar-nav">
              <li class="active"><a href="#/">Inicio</a></li>
              <li><a ng-href="#/empresa">Empresa</a></li>
            </ul>
 ``` 
 Y en el pie los scripts  
 
 ```bash
        <script src="scripts/app.js"></script>
        <script src="scripts/controllers/inicio.js"></script>
        <script src="scripts/controllers/empresa.js"></script>
  ```  
  - Luego iremos a la carpeta scripts--> controllers--> inicio.js donde cambiaremos lo que corresponda:  
```bash  
angular.module('zapatosApp')
  .controller('IniciotCtrl', function ($scope) {
    $scope.awesomeThings = [
      'HTML5 Boilerplate',
      'AngularJS',
      'Karma'
    ];
  });
  ```  
  Lo mismo en la carpeta scripts--> app.js  
  
  ```bash
  .config(function ($routeProvider) {
    $routeProvider
      .when('/', {
        templateUrl: 'views/inicio.html',
        controller: 'InicioCtrl'
      })
      .when('/empresa', {
        templateUrl: 'views/empresa.html',
        controller: 'EmpresaCtrl'
      })
      .otherwise({
        redirectTo: '/'
      });
  });
  ```  
  
