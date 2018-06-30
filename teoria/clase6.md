# Clase 6

### WebSockets

![WS_Sockets](http://fernetjs.com/wp-content/uploads/2012/11/websocket-lifecycle.png)
- Protocolo de comunicación qe se negocia sobre HTTP
- Full-duplex
- Única conexión permanente (Siempre conectado)
- Stream de mensajes
- Contenido en tiempo real
- Orientado a "eventos" (mensajes)
- Baja latencia


**Entendiendo los eventos**

![Sockets](http://cdn.sandsmedia.com/ps/onlineartikel/pspic/picture_file/53/WebSocket4dd23ade6df2a.png)


**Negociación del protocolo WebSocket**

> Para establecer una conexión WebSocket, el cliente manda una petición de negociación WebSocket, y el servidor manda una respuesta de negociación WebSocket, como se puede ver en el siguiente ejemplo:
> [WebSockets en Wikiwand](https://www.wikiwand.com/es/WebSocket)

- Cliente:
```
  GET /demo HTTP/1.1
  Host: example.com
  Connection: Upgrade
  Sec-WebSocket-Key2: 12998 5 Y3 1 .P00
  Sec-WebSocket-Protocol: sample
  Upgrade: WebSocket
  Sec-WebSocket-Key1: 4 @1 46546xW%0l 1 5
  Origin: http://example.com

  ^n:ds[4U
```

- Servidor:
```
  HTTP/1.1 101 WebSocket Protocol Handshake
  Upgrade: WebSocket
  Connection: Upgrade
  Sec-WebSocket-Origin: http://example.com
  Sec-WebSocket-Location: ws://example.com/demo
  Sec-WebSocket-Protocol: sample

  8jKS'y:G*Co,Wxa-
```
> Los 8 bytes con valores numéricos que acompañan a los campos Sec-WebSocket-Key1 y Sec-WebSocket-Key2 son tokens aleatorios que el servidor utilizará para construir un token de 16 bytes al final de la negociación para confirmar que ha leído correctamente la petición de negociación del cliente.


### WS: Nativo

- [Soporte](http://caniuse.com/#search=websocket)
- [Documentación en MDN](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket)

**Abrir la conexión**
```javascript
var myWebSocket = new WebSocket("ws://www.websockets.org");
```

**Gestión de Eventos**

- Siempre dispondremos del parametro event.
```javascript
myWebSocket.onopen = function() { console.log("Connection open ..."); };
myWebSocket.onmessage = function(evt) { console.log( "Received Message: ", evt.data); };
myWebSocket.onclose = function() { console.log("Connection closed."); };      
```

**Envio de mensajoes**
```javascript
myWebSocket.send("Hello WebSockets!");
```

**Cerrar la conexión**
```javascript
myWebSocket.close();
```

### WS: Sockets.io
![Sockets](https://camo.githubusercontent.com/b74075d1deca125ad36f8fded4055f896d9f2108/687474703a2f2f666f746f732e73756265666f746f732e636f6d2f32376135653361633065393936666134663063643066613239386635356366636f2e706e67)

Caracrerísticas:
- Fácil
- Soporte a navegadores obsoletos (Fallback)
- Popular
- Extraordinariamente simple

Eventos reservados:
- socket.on("connect", cb)
- socket.on("disconnect", cb)
- socket.on("error", cb) *Solo cliente*
- socket.on("message", cb)

**Abrir la conexión**
```javascript
  var socket = io("url");
```

**Gestión de Eventos**
```javascript
  socket.on('channel', function(data) { 
    console.log( "Received Message: ", data); 
  });
```

**Envio de mensajoes**
```javascript
socket.emit('channel', data);
```

**Cerrar la conexión**
```javascript
socket.disconnect() 
socket.close(); // Si quieres reabrir. -> socket.connect();
```

### Nativo vs. Librerías

- [Differences between socket.io and websockets en Stackoverflow](http://stackoverflow.com/a/38558531)
- [WebSocket and Socket.IO by DWD](https://davidwalsh.name/websocket)
- [An Introduction to WebSockets by Matt West](http://blog.teamtreehouse.com/an-introduction-to-websockets)


### Ejemplos

- [IT Pulse](https://github.com/UlisesGascon/twitter-sentiments)
- [Curratelo](https://github.com/UlisesGascon/curratelo)


### Geolocalización

- [Espeficicación](http://dev.w3.org/geo/api/spec-source.html)
- [Compatibildiad](http://caniuse.com/#feat=geolocation)

- Seguridad:
  - Necesario SSL
    - HTTPS
  - Confirmación del usuario

- Precisión:
    - Wi-fi (MAC)
    - Ethernet (IP)
    - Triangulación (3G y 4G)    
    - GPS (máxima precisión, pero tardará más en cargar)

- Métodos de *geolocation*
    - getCurrentPosition():
    ```javascript
        // Posición Actual
        navigator.geolocation.getCurrentPosition();
    ```
    - watchPosition():
    ```javascript
        // Seguimiento como un GPS
        navigator.geolocation.watchPosition();
    ```
    - clearWatch():
    ```javascript
        // Para el seguimiento
        navigator.geolocation.clearWatch();
    ```

- Propiedades de *geolocation*
    - Latitud (en base 10):
    ```javascript
        console.info(position.coords.latitude);
    ```
    - Longitud (en base 10):
    ```javascript
        console.info(position.coords.longitude);
    ```    
    - Precisión en posicionamiento:
    ```javascript
        console.info(position.coords.accuracy);
    ```    
    - Altitud (metros por encima del nivel del mar):
    ```javascript
        console.info(position.coords.altitude);
    ```    
    - Precisión de altitud:
    ```javascript
        console.info(position.coords.altitudeAccuracy);
    ```     
    - Rumbo (Grados respectos al norte):
    ```javascript
        console.info(position.coords.heading);
    ```     
    - Velocidad (m/s):
    ```javascript
        console.info(position.coords.speed);
    ```
    - Timestamp:
    ```javascript
        console.info(position.timestamp);
    ```


- Ajustes de *geolocation*

    - enableHighAccuracy:
        - Mejora los datos para conexiones no GPS, pero aumenta drásticamente el consumo de batería del dispositivo.
        - *False por defecto*

    - timeout:
        - Tiempo (ms) de espera antes de lanzar el error.
        - *0 por defecto*

    - maximumAge:
        - Tiempo (ms) para el almacenamiento en memoria cache de la posición actual
        - *0 por defecto*

    - Ejemplo:
    ```javascript
        var opciones = {
            enableHighAccuracy: true,
            maximumAge: 1000, // 1s
            timeout: 2000 // 2s
        }
    ```


- Trabajando con *geolocation*

    - Comprobando la compatibildiad de *geolocation*
    ```javascript
        if ("geolocation" in navigator) {
          console.info("Podemos usar Geolocation! :-) ");
        } else {
          console.warn("Geolocation no soportado :-( ");
        }
    ```

    - Probando la geolocalización:
    ```javascript
        if ("geolocation" in navigator) {
            navigator.geolocation.getCurrentPosition(function(position) {
                // Consola
                console.info("Latitud: " + position.coords.latitude + "\nLongitud: "+ position.coords.longitude);
                // HTML
                var datos = "<h1>Te pille!</h1>"
                datos += "Latitud: " + position.coords.latitude.toFixed(4) + "<br>"
                datos += "Longitud: "+ position.coords.longitude.toFixed(4)
                document.body.innerHTML = datos;
            });
        } else {
          console.warn("Geolocation no soportado :-( ");
        }
    ```
    - Mostrar la localización en una imagen estatica usando Gogole Maps:
    ```javascript
        if ("geolocation" in navigator) {
            navigator.geolocation.getCurrentPosition(function(position) {

                var latlon = position.coords.latitude + "," + position.coords.longitude;

                var img_url = "http://maps.googleapis.com/maps/api/staticmap?center="+latlon+"&zoom=14&size=400x300&sensor=false";

                document.body.innerHTML = "<img src='"+img_url+"'>";
            });
        } else {
          console.warn("Geolocation no soportado :-( ");
        }    
    ```

    - Gestionar los Errores y rechazos:
    ```javascript
    navigator.geolocation.getCurrentPosition(geo_success, geo_error);

    function geo_success(position) {
      console.info(position.coords.latitude+","+ position.coords.longitude);
    }

    function geo_error(error) {
        switch(error.code) {
            case error.PERMISSION_DENIED:
                document.body.innerHTML = "El usuario ha rechazado la petición.";
                console.warn(error);
                break;
            case error.POSITION_UNAVAILABLE:
                document.body.innerHTML = "La posición de usuario no es correcta.";
                console.warn(error);
                break;
            case error.TIMEOUT:
                document.body.innerHTML = "No hay respuesta en el tiempo limite previsto.";
                console.warn(error);
                break;
            case error.UNKNOWN_ERROR:
                document.body.innerHTML = "Error Desconocido";
                console.warn(error);
                break;
        }
    }
    ```   

    - Trabajando con ajustes personalizados:
    ```javascript
    navigator.geolocation.getCurrentPosition(geo_exito, geo_error, opciones);

    var opciones = {
        enableHighAccuracy: true,
        maximumAge: 1000, // 1s
        timeout: 2000 // 2s
    }

    function geo_exito(position) {
        console.info(position.coords.latitude+","+ position.coords.longitude);
    }

    function geo_error(error) {
        console.warn("Error! - "+error);
    }
    ```

    - Convirtiendo las coordenadas a hexadecimal:
    ```javascript

    /**
     * From Isabel Castillo
     * http://isabelcastillo.com/convert-latitude-longitude-decimal-degrees
     * Convert longitude/latitude decimal degrees to degrees and minutes
     * DDD to DMS, no seconds
     * @param lat, latitude degrees decimal
     * @param lng, longitude degrees decimal
     */

    function convertDMS( lat, lng ) {

            var convertLat = Math.abs(lat);
            var LatDeg = Math.floor(convertLat);
            var LatSec = (Math.floor((convertLat - LatDeg) * 60));
            var LatCardinal = ((lat > 0) ? "n" : "s");

            var convertLng = Math.abs(lng);
            var LngDeg = Math.floor(convertLng);
            var LngSec = (Math.floor((convertLng - LngDeg) * 60));
            var LngCardinal = ((lng > 0) ? "e" : "w");

            return LatDeg + LatCardinal + LatSec  + "<br />" + LngDeg + LngCardinal + LngSec;
    }
    ```

    - Sigue a un usuario:
    ```javascript

        navigator.geolocation.watchPosition(geo_exito, geo_error);

        function geo_exito(position) {
            console.info(position.coords.latitude +", "+ position.coords.longitude);
        }

        function geo_error(error) {
            console.warn("Error! - "+error);
        }

    ```

### ip-api.com

![Example](http://www.excelguru.ca/blog/wp-content/uploads/2014/07/image_thumb.png)

- [Web](http://ip-api.com/)
- [Docs](http://ip-api.com/docs/)
- [API](http://ip-api.com/docs/api:json)


### Carto

![Carto logo](https://carto.com/blog/img/posts/2016/2016-09-01-from-cartodb-to-carto/carto-logo.6e337a04.svg)

- [Carto](https://carto.com/)
- [Pricing](https://carto.com/pricing/)
- [Resources](https://carto.com/resources/)
- [Docs](https://carto.com/docs)
- [Guides](https://carto.com/learn/guides)
- [Blog](https://carto.com/blog)
- [CARTO en Github](https://github.com/CartoDB)

**Ejemplos**

- [Carto test by Robert Rouse](http://codepen.io/robertrouse/pen/Qdbzeb)
- [Galería de ejemplos](https://carto.com/gallery/)



### Google Maps

- [Portal informativo](https://developers.google.com/maps/?hl=Es)
- [Pricing](https://developers.google.com/maps/pricing-and-plans/?hl=Es)
- [Maps JavaScript API](https://developers.google.com/maps/documentation/javascript/?hl=Es)
    - [Tutorial](https://developers.google.com/maps/documentation/javascript/tutorial?hl=Es)
    - [API Ref.](https://developers.google.com/maps/documentation/javascript/3.exp/reference?hl=Es)
    - [Ejemplos](https://developers.google.com/maps/documentation/javascript/examples/?hl=Es)
- [Google Maps Embed API](https://developers.google.com/maps/documentation/embed/?hl=Es)
- [Google Street View Image API](https://developers.google.com/maps/documentation/streetview/?hl=Es)
- [Google Static Maps API](https://developers.google.com/maps/documentation/static-maps/?hl=Es)
- [Biblioteca JavaScript de Google Places API](https://developers.google.com/places/javascript/?hl=Es)

**Usarlo en tu proyecto**

- Librería
```html
<script type='text/javascript' src="http://maps.googleapis.com/maps/api/js?sensor=false&extension=.js&output=embed"></script>
```

- Centrar el mapa
```javascript
function initMap() {
  var map = new google.maps.Map(document.getElementById('map'), {
    zoom: 8,
    center: {lat: -3.8199647, lng: 40.4381307}
  });
}
```

- Markers ( [Demo](https://developers.google.com/maps/documentation/javascript/examples/marker-labels) )
```javascript
// In the following example, markers appear when the user clicks on the map.
// Each marker is labeled with a single alphabetical character.
var labels = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
var labelIndex = 0;

function initialize() {
  var bangalore = { lat: 12.97, lng: 77.59 };
  var map = new google.maps.Map(document.getElementById('map'), {
    zoom: 12,
    center: bangalore
  });

  // This event listener calls addMarker() when the map is clicked.
  google.maps.event.addListener(map, 'click', function(event) {
    addMarker(event.latLng, map);
  });

  // Add a marker at the center of the map.
  addMarker(bangalore, map);
}

// Adds a marker to the map.
function addMarker(location, map) {
  // Add the marker at the clicked location, and add the next-available label
  // from the array of alphabetical characters.
  var marker = new google.maps.Marker({
    position: location,
    label: labels[labelIndex++ % labels.length],
    map: map
  });
}

google.maps.event.addDomListener(window, 'load', initialize);
```

- Markers Personalizados ( [Demo](https://developers.google.com/maps/documentation/javascript/examples/icon-simple) )
```javascript
// This example adds a marker to indicate the position of Bondi Beach in Sydney,
// Australia.
function initMap() {
  var map = new google.maps.Map(document.getElementById('map'), {
    zoom: 4,
    center: {lat: -33, lng: 151}
  });

  var image = 'images/beachflag.png';
  var beachMarker = new google.maps.Marker({
    position: {lat: -33.890, lng: 151.274},
    map: map,
    icon: image
  });
}
```

- InfoWindows ( [Demo](https://developers.google.com/maps/documentation/javascript/examples/infowindow-simple) )
```javascript
// This example displays a marker at the center of Australia.
// When the user clicks the marker, an info window opens.

function initMap() {
  var uluru = {lat: -25.363, lng: 131.044};
  var map = new google.maps.Map(document.getElementById('map'), {
    zoom: 4,
    center: uluru
  });

  var contentString = '<div id="content">'+
      '<div id="siteNotice">'+
      '</div>'+
      '<h1 id="firstHeading" class="firstHeading">Uluru</h1>'+
      '<div id="bodyContent">'+
      '<p><b>Uluru</b>, also referred to as <b>Ayers Rock</b>, is a large ' +
      'sandstone rock formation in the southern part of the '+
      'Northern Territory, central Australia. It lies 335&#160;km (208&#160;mi) '+
      'south west of the nearest large town, Alice Springs; 450&#160;km '+
      '(280&#160;mi) by road. Kata Tjuta and Uluru are the two major '+
      'features of the Uluru - Kata Tjuta National Park. Uluru is '+
      'sacred to the Pitjantjatjara and Yankunytjatjara, the '+
      'Aboriginal people of the area. It has many springs, waterholes, '+
      'rock caves and ancient paintings. Uluru is listed as a World '+
      'Heritage Site.</p>'+
      '<p>Attribution: Uluru, <a href="https://en.wikipedia.org/w/index.php?title=Uluru&oldid=297882194">'+
      'https://en.wikipedia.org/w/index.php?title=Uluru</a> '+
      '(last visited June 22, 2009).</p>'+
      '</div>'+
      '</div>';

  var infowindow = new google.maps.InfoWindow({
    content: contentString
  });

  var marker = new google.maps.Marker({
    position: uluru,
    map: map,
    title: 'Uluru (Ayers Rock)'
  });
  marker.addListener('click', function() {
    infowindow.open(map, marker);
  });
}
```


### GeoJSON

- [Especificación](http://geojson.org/geojson-spec.html)
- [Web oficial](http://geojson.org/)
- [Ejemplo de uso en Github](https://github.com/timwaters/geojsonpastie/blob/master/test_places.fixture.geojson)

**Ejemplos**
```json
  { "type": "FeatureCollection",
    "features": [
      { "type": "Feature",
        "geometry": {"type": "Point", "coordinates": [102.0, 0.5]},
        "properties": {"prop0": "value0"}
        },
      { "type": "Feature",
        "geometry": {
          "type": "LineString",
          "coordinates": [
            [102.0, 0.0], [103.0, 1.0], [104.0, 0.0], [105.0, 1.0]
            ]
          },
        "properties": {
          "prop0": "value0",
          "prop1": 0.0
          }
        },
      { "type": "Feature",
         "geometry": {
           "type": "Polygon",
           "coordinates": [
             [ [100.0, 0.0], [101.0, 0.0], [101.0, 1.0],
               [100.0, 1.0], [100.0, 0.0] ]
             ]
         },
         "properties": {
           "prop0": "value0",
           "prop1": {"this": "that"}
           }
         }
       ]
     }
```

**Extras**

- [Validador/linter online](http://geojsonlint.com/)
- [Awesome Geojson](https://github.com/tmcw/awesome-geojson)

### Ejercicios


**1 -** Utiliza Google Maps para posicionar al usuario.

- [Solucion](https://codepen.io/ulisesgascon/pen/637fc481e9258728635361a369061e1c)


**2 -** Posiciona todos los vehículos de transporte (trenes y autorbuses) de Los Angeles en el mapa.

- [Información sobre la API de Metro.net](http://developer.metro.net/)
- [Utiliza un esquema de color personalizado](https://mapstyle.withgoogle.com/)
- [Snazzy, alternativa al sistema de estilos de Google Maps](https://snazzymaps.com/)
- [Utiliza Google Maps con un API Token](https://developers.google.com/maps/documentation/javascript/get-api-key?hl=ES)
- [Puedes utilizar Cluster de Google Maps](https://developers.google.com/maps/documentation/javascript/marker-clustering)

[Solución](http://codepen.io/ulisesgascon/full/XMJKQE/)


### LocalStorage

- [Soporte en Navegadores](http://caniuse.com/#search=localstorage)

- Convencional:
    - Cookies:
    - Espacio limitado (4kb)
    - Máximo 20 cookies
    - Por cada petición HTTP se envia y recibe todo el contenido (lectura servidor)
    - Caducidad

- Tipos:
    - SessionStorage (Solo se almacenan los datos durante la sesión)
    - LocalStorage (Almacenamiento persistente)
    - GlobalStorage (Firefox experimental - No usar)

- Uso y limitaciones:
    - 5-10Mb según navegador
    - Almacenamiento local (lectura/escritura cliente)
    - Sin caducidad
    - Funcionamiento clave/valor
    - Solo se permite el almacenamiento de cadenas de texto.


- Problemas Conocidos:
    - IE 8 y 9 almacena los datos basandose unicamente en el hostname, ignorando el protocolo (http/https) y el puerto.
    - En iOS 5 y 6 los datos se almacenan en una localización que puede ser borrada ocasionalmente por el SO.
    - Usando el modo "navegación privada" Safari, Safari para IOs y Navegadores Android no soportan sessionStorage o localStorage.
    - En IE al acceder a LocalStorage desde local, el objeto localStorage pasa a ser undefined.
    - Internet Explorer no soporta el almacenamiento de casi ninguna cadena de carácteres ASCII con una logitud menor a 20.
    - En IE el evento "storage" genera errores:
        - IE10 : Se dispara el evento al usar iframes.
        - IE11 : Se confunden el valor antiguo con el nuevo valor (actualizado).
    - IE10 en Windows 8 puede presentar el siguiente mensaje de error  "SCRIPT5: Access is denied" if "integrity" settings are not set correctly.
    - Chrome en Local no funciona correctamente

- Métodos de *LocalStorage*
    - setItem() Guardando datos
    ```javascript
        localStorage.setItem('clave', 'valor');
    ```

    - getItem() Recuperar un valor
    ```javascript
        console.info(localStorage.getItem('clave'));
    ```

    - length() Total de elementos
    ```javascript
        console.info(localStorage.length);
    ```

    - removeItem() Eliminar un elemento
    ```javascript
        localStorage.removeItem('clave');
    ```

    - clear() Eliminar todo
    ```javascript
        localStorage.clear();
    ```

- Comprobación
```javascript
    if (window.localStorage) {
        console.info("La función Html5 localStorage está soportada");
    } else {
        console.warn('Su navegador no soporta localStorage');
    }

    if (window.sessionStorage) {
        console.info('La función Html5 sessionStorage está soportada');
    } else {
        console.warn('Su navegador no soporta sessionStorage');
    }
```
- Usando json en LocalStorage
```javascript
    var objeto = {
        numero: 1,
        booleano : true,
        array: ["dato", true, 2],
        cadena: "dato"
        };
    localStorage.setItem('clave', JSON.stringify(objeto));
    var objetoRecuperado = JSON.parse(localStorage.getItem('clave'));
    console.log(objetoRecuperado.booleano);    
```

- Eventos asociados
    - key: es la clave que se modifica
    - oldValue: es el valor anterior de la clave que se modifica
    - newValue: es el nuevo valor de la clave que se modifica
    - url: la página donde se modifica la clave
    - storageArea: el objeto donde se modifica la clave
    - [Usa dos Pestañas](http://stackoverflow.com/questions/3055013/html5-js-storage-event-handler)
    - Ejemplo:
    ```javascript
        window.addEventListener('storage', function(event){
            console.info("Se registran cambios en "+event.key+". El valor pasó de ser "+event.oldValue+" a "+event.newValue+".\nRecuerda que estas en "+event.url+" y usando el almacenamiento ", event.storageArea);
        });
    ```

- Trucos:
    - Sacar todas las claves guardadas
   ```javascript
        for (var key in localStorage){
           console.log(key)
        }
    ```

    - Sacar todos los valores
    ```javascript
        for ( var i = 0; i < localStorage.length; i++ ) {
          console.log( localStorage.getItem( localStorage.key( i ) ) );
        }
    ```


### ContentEditable

- [Soporte en Navegadores](http://caniuse.com/#search=ContentEditable)

- Convencional:
    - Campos de texto
    - Formularios

- Comprobación:
```javascript
    if (!document.body.contentEditable) {
        console.warn("No se puede utilizar contentEditable :-(");
    } else {
        console.info("Podemos utilizar contentEditable :-)");
    }    
```
- Usando *contentEditable*
    - Atributo *contentEditable* (true, false, inherit)
    ```html
        <!-- estilos: [contenteditable] y [contenteditable]:hover -->
        <!DOCTYPE html>
        <html>
          <body>
            <div contentEditable="true">
              Modificame... tanto como quieras!
            </div>
          </body>
        </html
    ```
    - designMode (editando todo el documento)
    ```javascript
        window.document.designMode = "on"; // off, por defecto
    ```

    - [execCommand](https://developer.mozilla.org/es/docs/Web/API/Document/execCommand)
        - [Demo](http://www-archive.mozilla.org/editor/midasdemo/)
        - [Código](https://developer.mozilla.org/en-US/docs/Rich-Text_Editing_in_Mozilla)

    - spellcheck (editando todo el documento)
    ```html
        <!DOCTYPE html>
        <html>
          <body>
            <div contentEditable="true" spellcheck="true">
              Modificame... tanto como quieras!
            </div>
          </body>
        </html
    ```


### Ejercicios    

1 - Crear una libreta de contactos para guardar nombre y numero de telefono usando LocalStorage
- Objetivos:
    - Guardar Nombre y telefono
    - Mostrar todos los contactos en el html
    - Botón para borrar un contacto específico
    - Botón para borrar todos los contactos
    - Botón para recuperar el telefono de un contacto

```javascript
    //Tu solución
```

2 - Crea una libreta de contactos para guardar multiples datos.
- Objetivos:
    - Guardar Nombre, telefono y email
    - Mostrar todos los contactos en el html
    - Botón para borrar un contacto específico
    - Botón para borrar todos los contactos
    - Botón para recuperar el telefono y email de un contacto
- Consejos:
    - Utiliza *JSON.parse()* y *JSON.stringify()* para guardar multiples datos bajo una misma clave
    - Genera un avatar al azar para el usuario usando [Adorable Avatars](http://avatars.adorable.io/)

```javascript
    //Tu solución
```

### Offline

- [Soporte en Navegadores](http://caniuse.com/#search=offline%20web%20app)

- Uso y limitaciones:
    - Aplicación disponible independientemente del estado de la conexión
    - Se acelera la carga de los archivos
    - Disminuyen las consultas al servidor
    - En algunos navegadores es necesario que el usuario permita el almacenamiento
    - Para incluir cambios en la aplicación es necesario modificar el manifiesto


- Comprobación:
```javascript
    if (!window.applicationCache) {
        console.warn("No se puede utilizar applicationCache :-(");
    } else {
        console.info("Podemos utilizar applicationCache :-)");
    }
```

- Verificando la conexión:
```javascript
    if (window.navigator.onLine) {
        var detalles = "<h1>Estas Conectado a Internet!!</h1>";
        detalles += "<h3>Detalles del navegador:</h3>";
        detalles += "<p>CodeName: " + navigator.appCodeName + "</p>";
        detalles += "<p>Nombre: " + navigator.appName + "</p>";
        detalles += "<p>Versión: " + navigator.appVersion + "</p>";
        detalles += "<p>Cookies Habilitadas: " + navigator.cookieEnabled + "</p>";
        detalles += "<p>Lenguaje: " + navigator.language + "</p>";
        detalles += "<p>Plataforma: " + navigator.platform + "</p>";
        detalles += "<p>User-agent header: " + navigator.userAgent + "</p>";
        document.body.innerHTML = detalles;

    } else {
        document.body.innerHTML = "<h1>No estas Conectado!!</h1>"
        console.warn("No estamos conectados a Internet!");
    }
```
- Verificando la conexión usando eventos:
```javascript
    window.addEventListener("offline", function(){
        console.warn("Estas desconectado!")
    });

    window.addEventListener("online", function(){
        console.info("Estas conectado!")
    });
```

- Usando Cache (manifest):
    - Uso:
        - Los archivos son visibles en la pestaña Resources/Application Cache
        - Es necesario ajustar el MIME en algunos servidores
            ```
            // Ex: Apache
            AddType text/cache-manifest .appcache
            ```
        - El atributo manifest puede señalar a una URL pero deben tener el mismo origen que la aplicación web
        - Los sitios no pueden tener más de 5MB de datos almacenados en caché, pueden ser menos si el usuario lo cambia.
        - Si no se puede descargar el archivo de manifiesto o algún recurso especificado en él, fallará todo el proceso de actualización de la caché.
        - Añadir la versión del manifest como comentario.
        - JAMAS incluir el propio manifest dentro del manifest
        - Nuevo sistema de carga:
            - Si existe manifest, el navegador carga el documento y sus recursos asociados directamente desde local.
            - Se verifica si hubo actualizaciones al manifest.
            - Si se actualizo, el navegador descarga la nueva versión del archivo y de los recursos listados en él (segundo plano).
    - Estructura
        - CACHE, lo que se cacheará
        - NETWORK, lo que NO se cacheará
        - FALLBACK, que se visualizará si algo no esta disponible de manera offline

    - Incluyendo el manifest
    ```html
        <html manifest="ejemplo.appcache">
          <!-- ... -->
        </html>
    ```

    - Ejemplo de Manifest
    ```
        CACHE MANIFEST
        # versión 1.0

        # SI CACHEAR
        CACHE:
        index.html
        offline.html
        css/style.css
        js/script.js
        img1.jpg
        img2.jpg
        img3.jpg
        logo.png

        # Mostraremos offline.html cuando algo falle
        FALLBACK:
        offline.html

        # NO CACHEAR
        NETWORK:
        *
        # * es todo aquello que no este en CACHE
    ```

- Estados de Cache (manifest):
```javascript
    var appCache = window.applicationCache;

    switch (appCache.status) {
      case appCache.UNCACHED: // appCache.status == 0
        console.warn('Un objeto caché de la aplicación no se inicializó correctamente o falló.');
        break;
      case appCache.IDLE: // appCache.status == 1
        console.info('La caché no esta en uso.');
        break;
      case appCache.CHECKING: // appCache.status == 2
        console.info('El manifesto se ha obtenido y esta siendo revisado para actualizarse.');
        break;
      case appCache.DOWNLOADING: // appCache.status == 3
        console.info('Se estan descargando nuevos recursos debido a una actualización del manifesto.');
        break;
      case appCache.UPDATEREADY: // appCache.status == 4
        console.info('Hay una nueva versión del manifiesto.');
        break;
      case appCache.OBSOLETE: // appCache.status == 5
        console.info('El caché esta ahora obsoleto');
        break;
      default:
        console.warn('El Caché esta en estado desconocido');
        break;
    };
```    

- Eventos de Cache:
```javascript
function eventosCache(){

var appCache = window.applicationCache;
appCache.addEventListener('cached', chivato);
appCache.addEventListener('checking', chivato);
appCache.addEventListener('downloading', chivato);
appCache.addEventListener('error', chivato);
appCache.addEventListener('noupdate', chivato);
appCache.addEventListener('obsolete', chivato);
appCache.addEventListener('progress', chivato);
appCache.addEventListener('updateready', chivato);

    function chivato(e) {
        var conexion = (navigator.onLine) ? 'sí': 'no';
        var type = e.type;
        console.log('Conectado: ' + conexion+', Evento: ' + type +", \nMás Información: %O", e);
    }

}
```

- Forzar la actualización (manualmente):

```javascript
var appCache = window.applicationCache;

appCache.update(); // Intentamos actualizar la versión del usuario con un nuevo manifest

if (appCache.status == window.applicationCache.UPDATEREADY) {
  appCache.swapCache();  // La ctualización es correcta y se cambiado a la nueva versión
}
```

### Progressive Web Apps (PWAs)

- [Progressive Web Apps by Google Developers](https://developers.google.com/web/progressive-web-apps/)

**Manifest**
- [Banners de instalación de apps web](https://developers.google.com/web/fundamentals/engage-and-retain/app-install-banners/)
- [Web App Manifest](https://developer.mozilla.org/es/docs/Web/Manifest)
- [Notificaciones push en la web: Oportunas, relevantes y precisas](https://developers.google.com/web/fundamentals/engage-and-retain/push-notifications/)

### Notificationes

**Notifications API**

- [Notifications API - Soporte](http://caniuse.com/#feat=notifications)
- [Especificación](https://www.w3.org/TR/notifications/)

**Push API**

- [Push API - Soporte](http://caniuse.com/#feat=push-api)
- [Especificación](https://w3c.github.io/push-api/)

**Otros**

- [Sendpulse](https://sendpulse.com/features/webpush)
- [Onesignal](https://onesignal.com/webpush)
- [Agregado de notificaciones push a una app web](https://developers.google.com/web/fundamentals/getting-started/codelabs/push-notifications/)
- [PhoneGap Docs | Push Notifications](http://docs.phonegap.com/develop/push-notifications/)


### Web Workers: Intro

- Los Web Workers se ejecutan en un subproceso aislado.
- [Especificación](http://www.whatwg.org/specs/web-workers/current-work/)
- [Soporte](http://caniuse.com/#feat=webworkers)


**Arquitectura**
![ejemplo](https://az813057.vo.msecnd.net/testdrive/ieblog/2011/Jul/01_WebWorkersinIE10BackgroundJavaScriptMakesWebAppsFaster_2.png)


**Alcance**
- En el contexto de un Worker, tanto self como this hacen referencia al alcance global del Worker
- Puede acceder a:
	- Objeto navigator
	- Objeto location (de solo lectura)
	- XMLHttpRequest
	- setTimeout(), setInterval(), etc...
	- [Caché de la aplicación](https://developer.mozilla.org/es/docs/Web/API/Window/applicationCache)
	- Importación de secuencias de comandos externas, [importScripts()](https://developer.mozilla.org/en-US/docs/Web/API/WorkerGlobalScope/importScripts)
	- Generación de otros Web Workers

**Limitaciones**
- No se ejecutarán de forma local
- DOM
- Objeto window
- Objeto document
- Objeto parent


**Recomendaciones**

- Obtención previa y/o almacenamiento en caché de datos para un uso futuro
- Métodos para destacar la sintaxis de código u otros formatos de texto en tiempo real
- Corrector ortográfico
- Análisis de datos de vídeo o audio
- Entrada y salida en segundo plano o solicitud de servicios web
- Procesamiento de conjuntos o respuestas JSON de gran tamaño
- Filtrado de imágenes en <canvas>
- Actualización de varias filas de una base de datos web local

**Ejemplos**

- [Web Workers and Service Workers](http://codepen.io/ruzz311/pen/NNroab)
- [WEB Worker sample](http://codepen.io/nacholozano/pen/dpqvYk)

**Recursos**
- Librerías:
	- [parallel.js](https://github.com/parallel-js/parallel.js) 
	- [promise-worker](https://github.com/nolanlawson/promise-worker)
	- [Catiline.js](http://catilinejs.com/)
- [Introducción a los Web Workers en html5rocks](https://www.html5rocks.com/es/tutorials/workers/basics/)
- [Web workers without a separate Javascript file?](http://stackoverflow.com/questions/5408406/web-workers-without-a-separate-javascript-file)
- [Offline Recipes for Service Workers by DWB](https://davidwalsh.name/offline-recipes-service-workers)
- [Using Web Workers to Speed-Up Your JavaScript Applications by treehouse](http://blog.teamtreehouse.com/using-web-workers-to-speed-up-your-javascript-applications)
- [Debugging Web Workers with Chrome Developer Tools](https://blog.chromium.org/2012/04/debugging-web-workers-with-chrome.html)
- [Using Web Workers en MDN](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers)
- [Concurrency in JavaScript](http://typedarray.org/concurrency-in-javascript/)

### Web Workers: Básico

**Definición y puesta en marcha**

- Instanciando el Web Worker
```javascript
var worker = new Worker('fichero.js');
```

- Arrancando el Web Worker
```javascript
var worker = new Worker('fichero.js');
worker.postMessage();
```

**Comunicación**

- Desde el archivo principal
```javascript
var worker = new Worker('fichero.js');

worker.addEventListener('message', function(evento) {
  console.log('El worker dice: ', evento.data);
}, false);

worker.postMessage('Hola Mundo!');
```

- Desde el Web Worker
```javascript
self.addEventListener('message', function(evento) {
  self.postMessage(evento.data);
}, false);
```

**Parar un Web Worker**

- Desde el archivo principal
```javascript
worker.terminate()
```

- Desde el Web Worker
```javascript
self.close()
```

#### Web Workers: Avanzado

**Subworkers**
- La resolución de las URI de los Subworkers está relacionada con la ubicación de su Worker principal (en oposición a la página principal)
- Los Workers tienen la capacidad de generar Workers secundarios.
- Los Subworkers deben estar alojados en el mismo origen que la página principal.


**Blob**

- [Blob en MDN](https://developer.mozilla.org/es/docs/Web/API/Blob)
- [Soporte blob urls](http://caniuse.com/#feat=bloburls)
- [Soporte blob constructing](http://caniuse.com/#feat=blobbuilder)

En el HTML:
```html
<!-- inline worker -->
<script id="worker" type="javascript/worker">
    (function(s){
    	var increment = 1;
      var count = 0;
      var loop = 999999999;
      
      s.onmessage = function(e) {
          console.log(e.data);
          var test = 0;
          for( var i = 0 ; i < loop ;i++ ){
          	test = i;
            var int = Math.trunc( i*100/loop );
            if( int === count ){
          		s.postMessage( int );
              count += increment;
            }
          }
        s.postMessage( {finish:'loop finished'} );
      };
    })(self);
 </script>
```


En JS:
```javascript
var blob = new Blob([document.querySelector('#worker').textContent], { type: "text/javascript" });
var worker = new Worker(window.URL.createObjectURL(blob));
```

**Shared Web Workers**

- [Epecificación](https://html.spec.whatwg.org/multipage/workers.html#sharedworker)
- [Soporte Shared Web Workers](http://caniuse.com/#feat=sharedworkers)


**Definición y puesta en marcha**

- Instanciando el Web Worker
```javascript
var worker = new SharedWorker("fichero.js");
```

- Arrancando el Web Worker utiliznado port
```javascript
var worker = new SharedWorker("/html5/web-worker-shared.jsp");

worker.port.addEventListener("message", function(event) {
	console.log("datos del ShraedWorker", event.data);
}, false);

worker.port.start();
```

**Comunicación**

- Desde el archivo principal
```javascript
var worker = new SharedWorker("/html5/web-worker-shared.jsp");

worker.port.addEventListener("message", function(event) {
	console.log("datos del ShraedWorker", event.data);
}, false);

worker.port.postMessage("First Message");
```

- Desde el Web Worker
```javascript
var ports = [] ;

onconnect = function(event) {

    var port = event.ports[0];
    ports.push(port);
    port.start();

    port.addEventListener("message",
        function(event) { listenForMessage(event, port); } );
}


listenForMessage = function (event, port) {
    port.postMessage("Reply from SharedWorker to: " + event.data);
}

//Implementation of shared worker thread code
setInterval(function() { runEveryXSeconds() }, 5000);

function runEveryXSeconds() {
    for(i = 0; i < ports.length; i++) {
        ports[i].postMessage("Calling back at : "
                + new Date().getTime());
    }
}
```


### Ejercicio

**1 -** Utilizaremos la API de Blockchain.info para mostrar la informaión en tiempo real de las transacciones de Bitcoins.
- [Blockchain AP](https://blockchain.info/es/api)
- [Exchange Rates API](https://blockchain.info/es/api/exchange_rates_api)
- [Bitcoin Websocket API](https://blockchain.info/es/api/api_websocket)
- Suscribete a "unconfirmed_sub" y "blocks_sub"
- Filtra las operaciones tipo "utx"
- Convierte los [Satoshis en Bitcoins](https://aulabitcoin.com/basicos/que-es-un-satoshi/). 
- 1 BTC = 100,000,000 Satoshi

**Concepto**

![](https://i.giphy.com/3ohze2apsm6Qpb281y.gif)

```javascript
    //Tu solución
```