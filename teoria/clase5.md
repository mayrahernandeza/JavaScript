# Clase 5

### Eventos

- [Soporte](http://caniuse.com/#search=addEventListener)

- Podemos escuchar eventos y enlazar funciones (*event handler*)
![img_pro_bu](http://i.stack.imgur.com/liJ5u.png)
- [Demo](http://jsfiddle.net/L79xw5ye/)
- Propagación:
	- *Capturing* desde *document* hasta el elemento
	- *Target* impacta el elemento
	- *Bubbling* sube desde el elemento hasta *document*
	 

- Usando Eventos (Tradicional)
	- Solo una función por evento
	```html
		<button onclick="cambiarFondo()">Cambia el fondo</button>
	```
	
	```javascript
		function cambiarFondo() {
		
		// color = 'rgb(0-255,0-255,0-255'
		var color = 'rgb(' + Math.floor((Math.random() * 255))+ ',';
		color += Math.floor((Math.random() * 255)) + ',';
		color += Math.floor((Math.random() * 255)) + ')';
		
		document.body.style.backgroundColor= color;
		}
	```

- Usando Eventos (Callbacks)
	- Multiples funciones por evento
	- Necesidad de compatibilizar para IE8 
	```javascript
		// Callback - Manejador de Eventos
		function manejadorEventos(elEvento) {
		  	// Compatibilizar el evento
		  	var evento = elEvento || window.event;
		  	// Imprimir detalles
		  	console.log("-----------------------------")
		  	console.log("Type: "+evento.type); // Tipo
		  	console.log("Bubbles: "+evento.bubbles);
		  	console.log("Cancelable: "+evento.cancelable);
		  	console.log("CurrentTarget: ", evento.currentTarget);
			console.log("DefaultPrevented: "+evento.defaultPrevented);
			console.log("EventPhase: "+evento.eventPhase);
			console.log("Target: ", evento.target);
			console.log("TimeStamp: "+evento.timeStamp);
			console.log("IsTrusted: "+evento.isTrusted); // true - Usuario o false - Script
			console.log("=============================")
			// Desactivamos
			if (document.removeEventListener){ 
				document.removeEventListener('click', manejadorEventos, false);
				console.info("Listener quitado con exito");
			} else { // IE8
				document.detachEvent('onclick', manejadorEventos);
				console.info("Listener quitado con exito");
			}

		}
		
		// Añadimos Listener
		if (document.addEventListener){ 
			document.addEventListener('click', manejadorEventos, false);
			console.info("Listener añadido con exito");
		} else if (document.attachEvent){ // IE8
			document.attachEvent('onclick', manejadorEventos);
			console.info("Listener añadido con exito");
		} else {
			document.onclick = manejadorEventos;
			console.info("Listener añadido con exito");
		}
	```	

- Dominando el flujo
	- *.preventDefault()* evita el comportamiento por defecto (ex: Link -> nueva URL)
	- *.stopPropagation()* evita la propagación por el DOM (bubble) pero permite la acción por defecto.
	- *[Unable to understand useCapture attribute in addEventListener](http://stackoverflow.com/questions/7398290/unable-to-understand-usecapture-attribute-in-addeventlistener)*


- Gestión vs. Delegación de eventos
	- Gestión (asociar un evento por elemento)
	```html
		<ul id="miNav">
			 <li><a href="#nosotros">¿Quienes Somos?</a></li>
			 <li><a href="#objetivos">Los objetivos</a></li>
			 <li><a href="#equipo">Nuestro Equipo</a></li>
			 <li><a href="#detalles">Más detalles</a></li>
			 <li><a href="#contacta">Contactanos</a></li>
		</ul>
	```
	```javascript
	   var miNav = document.getElementById("miNav");
	   var miNavLinks = miNav.getElementsByTagName("a");
	   for (var i = 0; i < miNavLinks.length; i++) {
	     miNavLinks[i].onclick = function(){
	        console.info(this.innerHTML);
	     }
	   }
	```
	- Delegación (asociar un único evento al padre de los elementos)
	```html
		<ul id="miNav">
			 <li><a href="#nosotros">¿Quienes Somos?</a></li>
			 <li><a href="#objetivos">Los objetivos</a></li>
			 <li><a href="#equipo">Nuestro Equipo</a></li>
			 <li><a href="#detalles">Más detalles</a></li>
			 <li><a href="#contacta">Contactanos</a></li>
		</ul>
	```
	```javascript
	   var miNav = document.getElementById("miNav");
	   miNav.onclick = function(evento){
	      var evento = evento || window.event;
	      var elemento = evento.target || evento.srcElement;
	      console.info(elemento.innerHTML);
	   }
	```

### Programación dirigida a eventos

> La programación dirigida por eventos es un paradigma de programación en el que tanto la estructura como la ejecución de los programas van determinados por los sucesos que ocurran en el sistema, definidos por el usuario o que ellos mismos provoquen.

> Para entender la programación dirigida por eventos, podemos oponerla a lo que no es: mientras en la programación secuencial (o estructurada) es el programador el que define cuál va a ser el flujo del programa, en la programación dirigida por eventos será el propio usuario —o lo que sea que esté accionando el programa— el que dirija el flujo del programa. Aunque en la programación secuencial puede haber intervención de un agente externo al programa, estas intervenciones ocurrirán cuando el programador lo haya determinado, y no en cualquier momento como puede ser en el caso de la programación dirigida por eventos. [Wikiwand](https://www.wikiwand.com/es/Programaci%C3%B3n_dirigida_por_eventos)

- **Ejemplo:**
	```javascript
		var eventos = {
			agregar: null,
			quitar: null,
			manejador: function(evento) {
		        console.group("Manejador de Eventos");
			        console.log("-----------------------------");
		            console.log("Type: " + evento.type); // Tipo
		            console.log("Bubbles: " + evento.bubbles); // sube por el DOM
		            console.log("Cancelable: " + evento.cancelable);
		            console.log("CurrentTarget: ", evento.currentTarget);
		            console.log("DefaultPrevented: " + evento.defaultPrevented);
		            console.log("EventPhase: " + evento.eventPhase);
		            console.log("Target: ", evento.target);
		            console.log("TimeStamp: " + evento.timeStamp);
		            console.log("IsTrusted: " + evento.isTrusted); // true - Usuario o false - Script
		            console.log("=============================");
		        console.groupEnd();
		    }
		}
		
		// Init-time branching (Patrón)
		if (typeof window.addEventListener === 'function') {
		    eventos.agregar = function(el, type, fn) {
		    	el.addEventListener(type, fn, false);
			};
		    eventos.quitar = function(el, type, fn) {
		    	el.removeEventListener(type, fn, false);
		    };
		} else { // Soporte para IE8
		    eventos.agregar = function(el, type, fn) {
		        el.attachEvent('on' + type, fn);
		    };
		    eventos.quitar = function(el, type, fn) {
		        el.detachEvent('on' + type, fn);
		    };
		}
		
		eventos.agregar(document.body, 'click', function (e) {
			var color = 'rgb(' + Math.floor((Math.random() * 255))+ ',';
			color += Math.floor((Math.random() * 255)) + ',';
			color += Math.floor((Math.random() * 255)) + ')';
			document.body.style.backgroundColor= color;
			console.info("Nuevo color:", color);
		})
	```

### Eventos (custom)
```javascript
document.body.addEventListener("eventoCustom", function(e) {
	
	console.info("Detalles del evento: ", e);
	console.info("Información adiccional: ", e.detail);
	e.detail.metodo1();
});

var miEvento = new CustomEvent("eventoCustom", {
  detail: {
    dato1: "Hola!",
    metodo1: function(){
        console.log(this.dato1);
    }
  }
});

document.body.dispatchEvent(miEvento);
```

- [Soporte](http://caniuse.com/#search=customEvent)



### Ejercicios

**1 -** Hagamos la [web del Metro](https://www.metromadrid.es/es/index.html) más divertida.
- Saca el estado actual de todas las líneas del metro de Madrid por consola.

```javascript
	var lineas = document.querySelectorAll('.bloquet');
	for (var i = 0; i < lineas.length; i++) {
	  var estado = lineas[i].querySelector('.circulacion > .txt > a');
	  if(!estado) estado = lineas[i].querySelector('.circulacion > .r > a');
	  if(!estado) estado = lineas[i].querySelector('.circulacion > .n > a');
	  if(estado) console.log(estado.innerText.trim());
	}
```

**2 -**  Diseña un script que sustituya todas las imágenes de las entradas de [Tecnología del Mundo Today](http://www.elmundotoday.com/noticias/tecnologia/) por [imágenes dummy de gatitos](https://placekitten.com/).

```javascript
	//Tu solución
```

**3 -** Diseña un script que agrupe todos los titulares, sus autores y categorias dentro de [Genbeta:dev](http://www.genbetadev.com/) y luego vacíe el html para cargar una lista hecha por nosotros con la información previamente recolectada.

```javascript
	//Tu solución
```

### Trabajando con APIs

*CRUD*

- Create:
  - Method (POST):
    - Respuesta 200 - OK
    - Respuesta 204 - Sin contenido
    - Respuesta 404 - No encontrado
    - Respuesta 409 - Conflicto, ya existe
- Read:
  - Method (GET):
    - Respuesta 200 - OK
    - Respuesta 404 - No encontrado
- Update:
  - Method (PUT):
    - Respuesta 200 - OK
    - Respuesta 204 - Sin contenido
    - Respuesta 404 - No encontrado
- Delete:
  - Method (DELETE):
    - Respuesta 200 - OK
    - Respuesta 404 - No encontrado


![HTTP Protocolo](http://fundamentos-redes.wikispaces.com/file/view/3.3.2_Servicio_WWW_y_HTTP.jpg/255291246/960x432/3.3.2_Servicio_WWW_y_HTTP.jpg)

- Por tipología:
  - 1xx Informativas
  - 2xx Peticiones Correctas
  - 3xx Redirecciones
  - 4xx Errores Cliente
  - 5xx Errores Servidor
- [Lista de respuestas HTTP](https://es.wikipedia.org/wiki/Anexo:C%C3%B3digos_de_estado_HTTP)
- [Especificación](https://tools.ietf.org/html/rfc2616#section-10)

- Ejemplos:
	- [API RTVE](https://github.com/UlisesGascon/RTVE-API)

### AJAX

![Ajax comparación](http://gemsres.com/story/feb07/338111/fig1.jpg)

*Con Jquery*

```javascript
    function peticionJqueryAjax (url) {

	    $.ajax({
	        dataType: "json",
	        url: url,
	    })
	     .done(function( data, textStatus, jqXHR ) {
	         if ( console && console.log ) {
	             console.log( "La solicitud se ha completado correctamente." );
	             console.log( data );
	         }
	     })
	     .fail(function( jqXHR, textStatus, errorThrown ) {
	         if ( console && console.log ) {
	             console.log( "La solicitud a fallado: " +  textStatus);
	         }
	    });
	
	}
	
	peticionJqueryAjax ("<---URL---->");
```

*Vainilla JS*

- *Cambiando la cabecera*

Para cambiar datos en la cabecera se usa [.setRequestHeader()](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/setRequestHeader). Debe ser incluido antes de enviar la petición y después de abrir.

- *readyState*:
    - 0 es *uninitialized*
    - 1 es *loading*
    - 2 es *loaded*
    - 3 es *interactive*
    - 4 es *complete*

```javascript
    function peticionAjax(url) {
        var xmlHttp = new XMLHttpRequest();

        xmlHttp.onreadystatechange = function() {

            if (xmlHttp.readyState === 4 && xmlHttp.status === 200) {
                console.info(JSON.parse(xmlHttp.responseText));
            } else if (xmlHttp.readyState === 4 && xmlHttp.status === 404) {
                console.error("ERROR! 404");
                console.info(JSON.parse(xmlHttp.responseText));
            }
        };
        xmlHttp.open("GET", url, true);
        // Modificación de cabeceras
        xmlHttp.send();
    }

    peticionAjax("<---URL---->");
```

**Subir Imágenes**

```javascript
var imagen = document.getElementById("imagen-formulario").files[0];
var xhr = new XMLHttpRequest(); 
xhr.open("POST", url);
xhr.setRequestHeader("Content-Type", "image/png");
xhr.onload = function (detalles) { 
    // Terminado!
};
xhr.send(imagen);
```

### JSON

- JSON.parse()
  - Analiza la cadena y retorna los valores

- JSON.stringify()
  - Analiza los valores y retorna una cadena 

### JSONP

- json (formato)
  ```javascript
    { foo: 'bar' }
  ```
- callback (cliente)
  ```javascript
    mycallback = function(data){
      alert(data.foo);
    };
  ```
  
- peticion (cliente)
  ```javascript
    var url = "http://www.example.net/sample.aspx?callback=mycallback";
  ```
  
- respuesta (servidor)
  ```javascript
    mycallback({ foo: 'bar' });
  ```

- Ejemplo de CORS y JSONP con [php de formandome](http://www.formandome.es/javascript/cors-vs-jsonp-solicitudes-ajax-entre-dominios/)
  ```php
    <?php
    header('content-type: application/json; charset=utf-8');
    header("access-control-allow-origin: *");
     
    //Cadena de conexión:
    $connect = mysql_connect("localhost", "usuario", "pwd")
    or die('Could not connect: ' . mysql_error());
     
    //seleccionamos bbdd:
    $bool = mysql_select_db("database", $connect);
    if ($bool === False){
       print "No puedo encontrar la bbdd: $database";
    }
     
    //inicializamos el cliente en utf-8:
    mysql_query('SET names utf8');
   
    $query = "SELECT * FROM futbolistas";
     
    $result = mysql_query($query) or die("SQL Error: " . mysql_error());
    $data = array();
    // obtenemos los datos:
    while ($row = mysql_fetch_array($result, MYSQL_ASSOC)) {
        $data[] = array(
            'id' => $row['id'],
            'nombre' => $row['nombre'],
            'apellido' => $row['apellido'],
            'posicion' => $row['posicion'],
            'equipo' => $row['equipo'],
            'dorsal' => $row['dorsal'],
            'desc' => $row['desc'],
    	'imagen' => $row['imagen']
          );
    }
     
    //codificamos en json:
    $json = json_encode($data);
     
    //enviamos json o jsonp según venga o no una función de callback:
    echo isset($_GET['callback'])
        ? "{$_GET['callback']}($json)"
        : $json;
    ?>
  ```

Soporte en cliente (librerías):
- Jquery:
  ```javascript
    // Using YQL and JSONP
    $.ajax({
        url: "http://query.yahooapis.com/v1/public/yql",
     
        // The name of the callback parameter, as specified by the YQL service
        jsonp: "callback",
     
        // Tell jQuery we're expecting JSONP
        dataType: "jsonp",
     
        // Tell YQL what we want and that we want JSON
        data: {
            q: "select title,abstract,url from search.news where query=\"cat\"",
            format: "json"
        },
     
        // Work with the response
        success: function( response ) {
            console.log( response ); // server response
        }
    });  
  ```
- [jsonp.js de Przemek Sobstel](https://github.com/sobstel/jsonp.js/blob/master/jsonp.js)
- [J50Npi.js de Roberto Decurnex](https://github.com/robertodecurnex/J50Npi/blob/master/J50Npi.js)


### Fetch, una alternativa a XMLHttpRequest

- [Fetch API by DWB](https://davidwalsh.name/fetch)
- [Introduction to fetch() By Matt Gaunt](https://developers.google.com/web/updates/2015/03/introduction-to-fetch)
- [This API is so Fetching! by Mozilla Hacks](https://hacks.mozilla.org/2015/03/this-api-is-so-fetching/)
- [That's so fetch! by Jake Archibald](https://jakearchibald.com/2015/thats-so-fetch/)
- [Can I use? Fetch](http://caniuse.com/#search=fetch)


### CORS

- [Using CORS by html5rocks](https://www.html5rocks.com/en/tutorials/cors/)
- [Control de acceso HTTP (CORS) by MDN](https://developer.mozilla.org/es/docs/Web/HTTP/Access_control_CORS)
- [Cross-origin resource sharing en Wikipedia](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)
- [Enable cross-origin resource sharing](https://enable-cors.org/)
- [crossorigin.me (Proxy)](https://crossorigin.me/)

### Ejercicios

**1 -** Sacar en el html la respuesta de [OMDB](http://omdbapi.com/) para la pelicula Hackers

```javascript
	//Tu solución
```

**2 -** Sacar en el html el tiempo meteorológico de Madrid, Barcelona y Valencia. 
Nota: http://openweathermap.org te será de gran ayuda, busca la solución al error 401


```javascript
	//Tu solución
```

**3 -** Crea una web que nos permita ver los detalles almacenados en IMBD de una pelicula partiendo únicamente del título
Recursos:
- [OMDb API - The Open Movie Database](http://omdbapi.com/)

```javascript
	//Tu solución
```