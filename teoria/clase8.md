# Clase 8

### Iterator

> El patrón surge del deseo de acceder a los elementos de un contenedor de objetos (por ejemplo, una lista) sin exponer su representación interna. Además, es posible que se necesite más de una forma de recorrer la estructura siendo para ello necesario crear modificaciones en la clase.

> La solución que propone el patrón es añadir métodos que permitan recorrer la estructura sin referenciar explícitamente su representación. La responsabilidad del recorrido se traslada a un objeto iterador.

> El problema de introducir este objeto iterador reside en que los clientes necesitan conocer la estructura para crear el iterador apropiado. *[Wikipedia](https://www.wikiwand.com/es/Iterador_(patr%C3%B3n_de_dise%C3%B1o))*

- **Claves:**
	- Acceso secuencial a los elementos de un array o propiedades de un objeto sin exponerlos.

- **Array:**
```javascript
// Patrón Iterador
var iterador = (function() {

  var indice = 0,
      datos = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
      totalDatos = datos.length;

  return {
      siguiente: function() {
          var elemento;
          if (!this.tieneSiguiente()) {
              return null;
          }
          elemento = datos[indice];
          indice ++;
          return elemento;
      },
      tieneSiguiente: function() {
          return indice < totalDatos;
      },
      rebobinar: function() {
          indice = 0;
          return datos[indice];
      },
      actual: function() {
          return datos[indice];
      }
  };

}());

/* Probando
while(iterador.tieneSiguiente()) {  
    console.log(iterador.siguiente());
}

iterador.rebobinar();  
console.log(iterador.actual());  
*/
```

- **Objeto:**
```javascript
var iterador = (function() {

  var indice = 0,
      datos = { primerDato: 1, segundoDato: 'dos', tercerDato: 'tercero' },
      llaves = Object.keys(datos),
      totalDatos = llaves.length;

  return {
      siguiente: function() {
          var elemento;
          if (!this.tieneSiguiente()) {
              return null;
          }
          elemento = datos[llaves[indice]];
          indice ++;
          return elemento;
      },
      tieneSiguiente: function() {
          return indice < totalDatos;
      },
      rebobinar: function() {
          indice = 0;
          return datos[llaves[indice]];
      },
      actual: function() {
          return datos[llaves[indice]];
      }
  };

}());

/* Probando
while(iterador.tieneSiguiente()) {  
    console.log(iterador.siguiente());
}

iterador.rebobinar();  
console.log(iterador.actual());  
*/
```

### Façade

> Se aplicará el patrón fachada cuando se necesite proporcionar una interfaz simple para un subsistema complejo, o cuando se quiera estructurar varios subsistemas en capas, ya que las fachadas serían el punto de entrada a cada nivel. Otro escenario proclive para su aplicación surge de la necesidad de desacoplar un sistema de sus clientes y de otros subsistemas, haciéndolo más independiente, portable y reutilizable (esto es, reduciendo dependencias entre los subsistemas y los clientes). *[Wikipedia](https://www.wikiwand.com/es/Facade_(patr%C3%B3n_de_dise%C3%B1o))*

- **Claves:**
	- Creación de interfaces de alto nivel.
	- Muy usado en librerías
	- Oculta el código más complejo
	- Desacoplarnos de código externo

```javascript
var moduloRobotAutonomo = (function() {

    var _privado = {
        velocidad: 0,  // Km/h
        velocidadMax: 20, // Km/h
        velocidadMin: 2, // Km/h

        // ... más propiedades relativos a sensores, navegación, etc...

        velocidadActual: function() {
            console.log( "Velocidad Actual:" + _privado.velocidad);
        },
        ajustarVelocidad: function( valor ) {
            this.velocidad = valor;
        },
        acelerar: function() {
          if (_privado.velocidad >= _privado.velocidadMax ) {
            console.warn("Máxima velocidad Alcanzada!");
            _privado.velocidadActual();
          } else if (_privado.velocidad < _privado.velocidadMax){
              _privado.ajustarVelocidad (_privado.velocidad+1)
              _privado.velocidadActual();
          };
        },
        desacelerar: function() {
          if (_privado.velocidad <= _privado.velocidadMin ) {
            console.warn("Mínima velocidad Alcanzada!");
            _privado.velocidadActual();
          } else if (_privado.velocidad > _privado.velocidadMin){
              _privado.ajustarVelocidad (_privado.velocidad-1)
              _privado.velocidadActual();
          };
        },
        parar: function(){
          _privado.velocidad = 0;
          console.log("Robot parado");
        },
        validarVelocidad: function (valor) {
          if( valor <= _privado.velocidadMax && valor >= _privado.velocidadMin ){
            return true
          }else {
            return false
          }  
        }

		// más métodos relativos a sensores, navegación, etc...
    };

    return {
        facadeAPI: {
          velocidadCrucero: function(valor){
            if(_privado.validarVelocidad(valor)){
              _privado.ajustarVelocidad(valor);
              _privado.velocidadActual();
            }else{
              console.warn("La velocidad deseada "+valor+"Km/h no esta entre "+_privado.velocidadMin+"Km/h y los "+_privado.velocidadMax+"Km/h. permitidos" )
            }
          },
          masLento: _privado.desacelerar,
          masRapido: _privado.acelerar,
          stop:_privado.parar
        }
    };
}());

/* Jugando con el robot
var robot = moduloRobotAutonomo.facadeAPI;
robot.velocidadCrucero(20); // velocidad = 20
robot.masRapido(); // Max alcanzado
robot.stop(); // Parado
robot.masLento(); // Min alcanzado
*/
```

### Mediator

> Habitualmente un programa está compuesto de un número de clases (muchas veces elevado). La lógica y computación es distribuida entre esas clases. Sin embargo, cuantas más clases son desarrolladas en un programa, especialmente durante mantenimiento y/o refactorización, el problema de comunicación entre estas clases quizás llegue a ser más complejo. Esto hace que el programa sea más difícil de leer y mantener. Además, puede llegar a ser difícil cambiar el programa, ya que cualquier cambio podría afectar código en muchas otras clases.

> Con el patrón mediador, la comunicación entre objetos es encapsulada con un objeto mediador. Los objetos no se comunican de forma directa entre ellos, en lugar de ello se comunican mediante el mediador. Esto reduce las dependencias entre los objetos en comunicación, reduciendo entonces la Dependencia de código. *[Wikipedia](https://www.wikiwand.com/es/Mediator_(patr%C3%B3n_de_dise%C3%B1o))*

- Claves:
	- Una interfaz única - Objeto central.
	- Todos los módulos comunican con este módulo central.

```javascript
	// Patron de Mediador
	var moduloCentral = (function(){

	    var _temas = {};

	    var _suscribir = function( tema, fn ){
	        if ( !_temas[tema] ){
	            _temas[tema] = [];
	        }
	        _temas[tema].push( { context: this, callback: fn } );
	        return this;
	    };

	    var _publicar = function( tema ){
	        var args;
	        if ( !_temas[tema] ){
	            return false;
	        }
	        args = Array.prototype.slice.call( arguments, 1 );
	        for ( var i = 0, l = _temas[tema].length; i < l; i++ ) {
	            var subscription = _temas[tema][i];
	            subscription.callback.apply( subscription.context, args );
	        }
	        return this;
	    };

	    return {
	        verTemas: _temas,
	        publicar: _publicar,
	        suscribir: _suscribir,
	        instalarEn: function( obj ){
	            obj.suscribir = _suscribir;
	            obj.publicar = _publicar;
	        }
	    };

	}());

	// Creamos dos Objetos
	var modulo1 = {};
	var modulo2 = {};
	console.clear();

	// Instalamos ...
	moduloCentral.instalarEn(modulo1);
	moduloCentral.instalarEn(modulo2);

	// Ajustamos las suscripciones
	modulo1.suscribir("test", function(){
	    console.info("\"modulo1\" suscrito a \"test\". Callback disparado! ")
	});

	modulo2.suscribir("test2", function(){
	    console.info("\"modulo2\" suscrito a \"test2\". Callback disparado! ")
	});

	moduloCentral.suscribir("testCentral", function(){
	    console.info("\"moduloCentral\" suscrito a \"testCentral\". Callback disparado! ")
	});

	// Suscripciones extra...
	moduloCentral.suscribir("test", function(){
	    console.info("\"moduloCentral\" suscrito a \"test\". Callback disparado! ")
	});
	moduloCentral.suscribir("test2", function(){
	    console.info("\"moduloCentral\" suscrito a \"test2\". Callback disparado! ")
	});
	console.clear();

	// Disparamos las publicaciones
	modulo2.publicar("testCentral");
	modulo2.publicar("test2");
	modulo2.publicar("test");

	modulo1.publicar("testCentral");
	modulo1.publicar("test2");
	modulo1.publicar("test");

	moduloCentral.publicar("testCentral");
	moduloCentral.publicar("test2");
	moduloCentral.publicar("test");
```

### Mixins

> En los lenguajes de programación orientada a objetos, un mixin es una clase que ofrece cierta funcionalidad para ser heredada por una subclase, pero no está ideada para ser autónoma. Heredar de un mixin no es una forma de especialización sino más bien un medio de obtener funcionalidad. Una subclase puede incluso escoger heredar gran parte o el total de su funcionalidad heredando de uno o más mixins mediante herencia múltiple.

> Un mixin puede aplazar la definición y la vinculación de métodos hasta el tiempo de ejecución, aunque los atributos y los parámetros de instanciación siguen siendo definidos en tiempo de compilación. [Wikipedia](https://www.wikiwand.com/es/Mixin)

```javascript
    var Perro = function(nombre) {
        this.nombre = nombre  || "Sin nombre aún"
        this.patas = 4;
        this.ojos = 2;
    };

    // Mixin 1
    var PerroGuia = function() {};

    PerroGuia.prototype = {
        guiar: function() {
            console.log("Te estoy guiando...");
        }
    };

    // Mixin 2
    var PerroSuperPoderes = function() {};

    PerroSuperPoderes.prototype = {
    	perseguir: function() {
            console.log("Te estoy persiguiendo....");
        },
        rastrear: function() {
            console.log("Te estoy rastreando...");
        },
        camuflar: function() {
            console.log("Ya no me ves...");
        },
        conducir: function() {
            console.log("Ahora... estoy conduciendo!");
        }
    };

    // Mixin 3
    var PastorAleman = function () {};

    PastorAleman.prototype = {
        colorLengua: "negra",
        colorOjos: "marrón",
        capacidadTrabajo: true,
        especialidad: "Pastoreo"
    };


    function extender(claseReceptora, claseDonante) {
        // solo extendemos los metodos que pasamos por parametros
        if (arguments[2]) {
            for (var i = 2, len = arguments.length; i < len; i++) {
                claseReceptora.prototype[arguments[i]] = claseDonante.prototype[arguments[i]];
            }
        }
        // extendemos todos los metodos
        else {
            for (var nombreMetodo in claseDonante.prototype) {
                // comprobamos que ya no existiese un metodo llamado igual
                if (!claseReceptora.prototype[nombreMetodo]) {
                    claseReceptora.prototype[nombreMetodo] = claseDonante.prototype[nombreMetodo];
                }
            }
        }
    }

    // Extendemos todos los metodos
    extender(Perro, PerroGuia);
    extender(Perro, PastorAleman);

    // Extendemos solo conducir
    extender(Perro, PerroSuperPoderes, "conducir");

    var miPerroGuia = new Perro("K9");

    // utilizamos los metodos heredados
    console.log("Nombre: "+miPerroGuia.nombre+"\nNúmero patas: "+miPerroGuia.patas+"\n Número ojos: "+miPerroGuia.ojos+"\n Color Lengua: "+miPerroGuia.colorLengua+"\n Color ojos: "+miPerroGuia.colorOjos+"\n Capacidad de trabajo: "+miPerroGuia.capacidadTrabajo+"\n Especialidad: "+miPerroGuia.especialidad);
    miPerroGuia.guiar();
    miPerroGuia.conducir();
```

### Observer

> Definir una dependencia uno-a-muchos entre objetos, de tal forma que cuando el objeto cambie de estado, todos sus objetos dependientes sean notificados automáticamente. Se trata de desacoplar la clase de los objetos clientes del objeto, aumentando la modularidad del lenguaje, creando las mínimas dependencias y evitando bucles de actualización (espera activa o polling). En definitiva, normalmente, usaremos el patrón Observador cuando un elemento “quiere” estar pendiente de otro, sin tener que estar encuestando de forma permanente si éste ha cambiado o no.

> El patrón Observer es la clave del patrón de arquitectura Modelo Vista Controlador (MVC). *[Wikipedia](https://www.wikiwand.com/es/Observer_(patr%C3%B3n_de_dise%C3%B1o))*

```javascript
    var objetoPrincipal = {
        observadores: [],

        suscripcion: function(f) {
            this.observadores.push(f);
        },

        eliminarSuscripcion: function(f) {
            this.observadores = this.observadores.filter(
                function(observador) {
                    if (observador !== f) {
                        return observador;
                    }
                }
            );
        },

        mensaje: function(o, objeto) {
            var ambito = objeto || window;
            this.observadores.forEach(function(observador) {
                observador.call(ambito, o);
            });
        }
    }

    var otraFuncion1 = function(item) {
        console.info(item + " desde otraFuncion1");
    };

    function otraFuncion2(item) {
        console.info(item + " desde otraFuncion2");  
    };

    // Pruebas
    objetoPrincipal.mensaje('mensaje #1');

    objetoPrincipal.suscripcion(otraFuncion1);
    objetoPrincipal.suscripcion(otraFuncion2);
    objetoPrincipal.mensaje('mensaje #2');

    objetoPrincipal.eliminarSuscripcion(otraFuncion2);
    objetoPrincipal.mensaje('mensaje #2');

    objetoPrincipal.suscripcion(otraFuncion2);
    objetoPrincipal.mensaje('mensaje #3');
```

### Chain of Responsability

> El patrón de diseño Chain of Responsibility es un patrón de comportamiento que evita acoplar el emisor de una petición a su receptor dando a más de un objeto la posibilidad de responder a una petición. Para ello, se encadenan los receptores y pasa la petición a través de la cadena hasta que es procesada por algún objeto. Este patrón es utilizado a menudo en el contexto de las interfaces gráficas de usuario donde un objeto puede contener varios objetos. Según si el ambiente de ventanas genera eventos, los objetos los manejan o los pasan. [Wikipedia](https://www.wikiwand.com/es/Chain_of_Responsibility_(patr%C3%B3n_de_dise%C3%B1o))

```javascript
    var Peticion = function(cantidad) {
        this.cantidad = cantidad;
        logger.registra("Pedido: " + cantidad + "€\n");
    }

    Peticion.prototype = {
        get: function(valorMoneda) {
            var cuenta = Math.floor(this.cantidad / valorMoneda);
            this.cantidad -= cuenta * valorMoneda;

    		if(cuenta !== 0){
    	        if(valorMoneda < 5 ) {
    				logger.registra("Facilita un total de " + cuenta + " monedas de " + valorMoneda + "€");
    	        } else {
    	        	logger.registra("Facilita un total de " + cuenta + " billetes de " + valorMoneda + "€");
    	        }
    		}

            return this;
        }
    }

    var logger = (function() {
        var registro = "";
        return {
            registra: function(mensaje) { registro += mensaje + "\n"; },
            resumen: function() {
            	console.log(registro);
            	registro = "";
            }
        }
    })();

    function calcularBilletes(cantidad) {
        var peticion = new Peticion(cantidad);

        peticion.get(500).get(200).get(100).get(50).get(20).get(10).get(5) // Billetes
        		.get(2).get(1).get(0.50).get(0.20).get(0.10).get(0.05).get(0.02).get(0.01); // Monedas

        logger.resumen();
    }

    calcularBilletes(443.79);
```

### MVC Convencional

![MVC Convencional](http://cdn.infoq.com/statics_s1_20151222-0055/resource/news/2014/05/facebook-mvc-flux/en/resources/flux-react-mvc.png)

- [Aproximación de Alex Netkachov](https://alexatnet.com/articles/model-view-controller-mvc-javascript)
- [Ejemplo de Alex Netkachov](http://jsfiddle.net/alex_netkachov/ZgBrK/)




### Monitorizar Variables Globales

- [Detectar variables globales en Javascript por Carlos Benítez](http://www.etnassoft.com/2011/04/04/detectar-variables-globales-en-javascript/)
```javascript
    /**
    * Snippet Detectar variables globales
    * en Javascript por Carlos Benítez
    */
    (function( context ){
      var globals = { viewGlobals : true },
          startGlobals = [],
          newGlobals = [];

      for (var j in window) {
        globals[j] = true;
        startGlobals.push(j);
      }

      setInterval(function() {
        for ( var j in window ) {
          if ( !globals[j] ) {
            globals[j] = true;
            newGlobals.push(j);
            console.warn( 'New Global: ' + j + ' = ' + window[j] + '. Typeof: ' + (typeof window[j]) );
          }
        }
      }, 1000);

      context.viewGlobals = function(){
        console.groupCollapsed( 'View globals' );
          console.groupCollapsed( 'Initial globals' );
            console.log( startGlobals.sort().join( ",\n" ) );
          console.groupEnd();
          console.groupCollapsed( 'New globals' );
            console.warn( newGlobals.sort().join( ",\n" ) );
          console.groupEnd();
        console.groupEnd();
      };

    })(this);
```


### Colas de Ejecución

- Ejemplo (modificado) de Valentín Starck:
    - [Versión con comentarios](http://blog.aijoona.com/2011/04/30/implementando-colas-de-ejecucion-en-javascript/)
```javascript

    function write(t) {
      console.log(new Date + ' | ' +t);
    }

    var Queue = function() {
      this._tasks = [];
    };

    Queue.prototype.add = function(fn, scope) {
      this._tasks.push({
        fn: fn,
        scope: scope
      });
      return this;
    };

    Queue.prototype.process = function() {
      var proxy, self = this;

      task = this._tasks.shift();

      if(!task) {
        return;
      }

      proxy = {
        end: function() {
          self.process();
        }
      };

      task.fn.call(task.scope, proxy);

      return this;    
    };

    Queue.prototype.clear = function() {
      this._tasks = [];

      return this;
    };

    var q = new Queue();

    // Tarea 1 (sincronica)
    q.add(function(proxy) {
      write('Tarea 1 (sincronica)');
      proxy.end();
    });

    // Tarea 2 (asincronica)
    q.add(function(proxy) {
      write('Tarea 2 (asincronica)');
      setTimeout(function() {
        proxy.end();        
      }, 2500);
    });

    // Tarea 3 (sincronica)
    q.add(function(proxy) {
      write('Tarea 3 (sincronica)');
      proxy.end();
    });

    // Tarea 4 (asincronica)
    q.add(function(proxy) {
      write('Tarea 4 (asincronica)');
      setTimeout(function() {
        proxy.end();        
      }, 2500);
    });

    // Tarea 5 (sincronica)
    q.add(function(proxy) {
      write('Tarea 5 (sincronica)');
      proxy.end();
    });

    q.process();
```
- [Librería QueQue de Valentín Starck](https://github.com/Aijoona/QueQue)


### Callback Hell
-![callback_humor](http://cdn.meme.am/instances/400x/63888921.jpg)
- Callback Hell
    - Escalabilidad limitada
    - Problemas para gestionar errores
    - Anidaciones complejas
    ```javascript
    obj.metodo1(data, function(data1) {  
        function(data1, function(data2) {
            obj.otroMetodo(data2, function(data3) {
                obj.otroMetodoMas(data3, function(data4) {
                    // más y más.....
                })
            })
        })
    })

    ```

### Promesas

![promises_ecma6](https://mdn.mozillademos.org/files/8633/promises.png)
> A promise represents the eventual result of an asynchronous operation. The primary way of interacting with a promise is through its *then* method, which registers callbacks to receive either a promise’s eventual value or the reason why the promise cannot be fulfilled.
> From [promisesaplus.com/](http://promisesaplus.com/)

- Estados:
    - Fulfilled – La acción en relación con la promesa se logró.
    - Rejected – La acción en relación con la promesa falló.
    - Pending – Pendiente, no se ha cumplido o rechazado aún.
    - Settled - Arreglada, se ha cumplido o se ha rechazado (resuelta).

- En ECMA6:
- [Soporte en navegadores](http://caniuse.com/#search=promise)  
    - Una promesa
    ```javascript
        var cuentaPromesas = 0;
        var errorMode = false;
        function testPromesa() {

          var numPromesaActual = ++cuentaPromesas;

          console.warn("Promesa Asíncrona numero ("+numPromesaActual+") - Iniciada")

          var miPromesa = new Promise(
            function(resolve, reject) {       

              console.info("Promesa Asíncrona numero ("+numPromesaActual+") - Proceso asincrónico empezado");

              if(errorMode){
                  reject(numPromesaActual)
              } else{
                window.setTimeout(
                  function() {
                    resolve(numPromesaActual)
                  }, Math.random() * 2000 + 1000);
              }
            });
          miPromesa.then(
            function(val) {
              console.info("Promesa Asíncrona numero ("+val+") - Proceso asincrónico terminado");
              console.warn("Promesa Asíncrona numero ("+numPromesaActual+") - Finalizada");
            }).catch(
              function(val){
                console.error("Promesa Asíncrona numero ("+val+") - ERROR FATAL");
            });
        };

        testPromesa();
    ```
    - Usando *.race()*
    ```javascript   
        var p1 = new Promise(function(resolve, reject) {
            setTimeout(resolve, 500, "uno");
        });
        var p2 = new Promise(function(resolve, reject) {
            setTimeout(resolve, 100, "dos");
        });

        Promise.race([p1, p2]).then(function(value) {
          console.log(value); // "dos" - p2 más rápida
        });
    ```
    - Usando *.all()*
    ```javascript
        var errorMode = false

        var p1 = new Promise(function(resolve, reject) {
          console.log("P1 - Iniciada");
          setTimeout(resolve, 1000, "P1 - Terminada");
        });
        var p2 = new Promise(function(resolve, reject) {
          console.log("P2 - Iniciada");
          setTimeout(resolve, 2000, "P2 - Terminada");
        });
        var p3 = new Promise(function(resolve, reject) {
          if(errorMode){
            reject("errorMode-Activado");
          } else {
            console.log("P3 - Iniciada");
            setTimeout(resolve, 3000, "P3 - Terminada");
          }

        });
        var p4 = new Promise(function(resolve, reject) {
          console.log("P4 - Iniciada");
          setTimeout(resolve, 4000, "P4 - Terminada");
        });

        Promise.all([p1, p2, p3, p4]).then(function(value) {
          console.info("Promise.all -> TODO TERMINADO", value)
        }, function(reason) {
          console.log("Parece... que fallamos!", reason)
        });
    ```
    - Usando Ajax
    ```javascript
        function $http(url){

          var core = {

            ajax : function (method, url, args) {

              var promise = new Promise( function (resolve, reject) {

                var client = new XMLHttpRequest();
                var uri = url;

                if (args && (method === 'POST' || method === 'PUT')) {
                  uri += '?';
                  var argcount = 0;
                  for (var key in args) {
                    if (args.hasOwnProperty(key)) {
                      if (argcount++) {
                        uri += '&';
                      }
                      uri += encodeURIComponent(key) + '=' + encodeURIComponent(args[key]);
                    }
                  }
                }

                client.open(method, uri);
                client.send();

                client.onload = function () {
                  if (this.status >= 200 && this.status < 300) {
                    resolve(this.response);
                  } else {
                    reject(this.statusText);
                  }
                };
                client.onerror = function () {
                  reject(this.statusText);
                };
              });

              return promise;
            }
          };

          // Patrón Adaptador
          return {
            'get' : function(args) {
              return core.ajax('GET', url, args);
            },
            'post' : function(args) {
              return core.ajax('POST', url, args);
            },
            'put' : function(args) {
              return core.ajax('PUT', url, args);
            },
            'delete' : function(args) {
              return core.ajax('DELETE', url, args);
            }
          };
        };

        var callback = {
          success : function(data){
             console.log(1, 'success', JSON.parse(data));
          },
          error : function(data){
             console.log(2, 'error', JSON.parse(data));
          }
        };

        // Prueba bicis
        $http("http://bicimad-api.herokuapp.com/api-v1/locations/")
          .get()
          .then(callback.success)
          .catch(callback.error);

        // Prueba bicis 2
        var bicisCerca = {
          url: 'http://bicimad-api.herokuapp.com/api-v1/locations/nearest/',
          lat: '40.418889',
          long: '-3.691944',
          distance: '1000000000'
        };
        var cercaMio = bicisCerca.url+"?lat="+bicisCerca.lat+"&long="+bicisCerca.long+"&distance="+bicisCerca.distance;

        $http(cercaMio)
          .get()
          .then(callback.success)
          .catch(callback.error);
    ```

- Alternativas para usar promesas:
    - [Q](https://github.com/kriskowal/q)
        - Muy utilizada en Nodejs
        - [Q.js](https://rawgit.com/kriskowal/q/v1/q.js);
        - [Q Docs](https://github.com/kriskowal/q/wiki/API-Reference)
        ```javascript
            function primeraFuncion() {
                var deferred = Q.defer();
                setTimeout(function() {
                    console.info('Primera funcion');
                    deferred.resolve();
                }, 2000);
                return deferred.promise;
            }

            function segundaFuncion() {
                var deferred = Q.defer();
                setTimeout(function() {
                    console.info('Segunda funcion');
                    deferred.resolve();
                }, 1000);
                return deferred.promise;
            }
            Q()
                .then(primeraFuncion)
                .then(segundaFuncion)
                .done();        
        ```

    - [RSVP.js](https://github.com/tildeio/rsvp.js)
    - [When.js](https://github.com/cujojs/when)



### Más Patrones

- [Colección de patrones de Javascript de Shi Chuan](https://github.com/shichuan/javascript-patterns)
- [Libro "Essential Javascript Design Patterns" de Addy Osmani](https://github.com/addyosmani/essential-js-design-patterns)
- [Libro "JavaScript Patterns" de Stoyan Stefanov](http://shop.oreilly.com/product/9780596806767.do)


### Algoritmia

- [Introducción al diseño de algoritmos en Javascript](http://www.etnassoft.com/2011/07/06/introduccion-al-diseno-de-algoritmos-en-javascript/)
- [Algorithm Visualizer](https://github.com/parkjs814/AlgorithmVisualizer)
- [Minimal and clean examples of machine learning algorithms](https://github.com/rushter/MLAlgorithms)
- [Torch implementation of neural style algorithm](https://github.com/jcjohnson/neural-style)
- [Collection of classic computer science paradigms, algorithms, and approaches written in JavaScript.](https://github.com/nzakas/computer-science-in-javascript)
- [JavaScript implementation of different computer science algorithms](https://github.com/mgechev/javascript-algorithms)
- [Atwood's Law applied to CS101 - Classic algorithms and data structures implemented in JavaScript](https://github.com/felipernb/algorithms.js)



### Patrones y Arquitecturas alternativas

- **[GoblinDB](GoblinDBRocks.github.io)**
  - Base de datos reactiva
  - Almacenamiento Asíncrono
  - Patrones de diseño (Namespace, Façade, etc...)
  - Ambush Functions, funciones Lambda a demanda
  - Soporte a Eventos

- **[Simple hangouts bot](https://github.com/UlisesGascon/simple-hangouts-bot)**
  - Aislamiento del core para mejorar la portabilidad usando patrones
  - XMPP Protocolo
  - Soporte de operaciones en terminal
  - Soporte para la instalación como dependencia de NPM
  - Extensión de por API interna
  - Incorporación de servicios externos como Alchemy (Inteligencia Artificial como servicio)
  - Gestión de la asincronía
  - Array de objetos
  - Gestión de notificación y ayuda al usuario
  - Detección de eventos de Error y cierre del sistema

- **[The scraping machine](https://github.com/UlisesGascon/the-scraping-machine)**
  - Alto nivel de abstracción para el usuario final
  - Soporte como aplicación de terminal con [Vorpal](https://www.npmjs.com/package/vorpal)
  - Generación dinámica de scripts en varios lenguajes (JS, Python, etc...)
  - Gestión de procesos hijos de forma nativa
  - Instalación global como módulo de NPM

- **[GingerCode](https://github.com/GingerCode)**
  - Orientado a nuevos programadores
  - Pseudocódigo funcional
  - Alto nivel de abstracción
  - Isomórfico

- **[OSWaldito](https://github.com/OSWeekends/OSWaldito)**
  - Orientado a IOT
  - Comunciación I2C
  - Movimiento controlado por WebSockets
  - Renderización en cliente de VR usando three.js
  - Stream de vídeo bajo demanda frame a frame
  - Uso del sintetizador de voz nativo de Chrome
  - Gestión de redes sociales

- **[Slack Canal Directo](https://github.com/OSWeekends/Slack-Canal-Directo)**
  - Orientado a la gestión de redes sociales
  - Escucha activamente conversaciones en Google Hangouts
  - Envía mensajes en Google Hangouts
  - Envía mensajes en Slack
  - Envía mensajes al azar clasificados por prioridad en Slack
  - Envía mensajes de Error y estado al administrador en Goolgle Hangouts
  - Puede ser desplegado en multiples entornos (Raspbian, Linux, OSX, Windows, C9...)
  - Permite desplegar multiples avatares y personalidades desde la configuración para comunicarse en Slack

- **[Know Your SNPs](https://github.com/OSWeekends/know-your-SNPs)**
  - Proyecto BioTecnológico
  - Analiza ADN
  - Permite buscar dentro del ADN ciertos patrones
  - No almacena datos
  - Futura migración a aplicación de escritorio
  - Formulario para realizar nuevas queries (desarrollo) sin tener que programar

- **[protoUnicorn](https://github.com/OSWeekends/protoUnicorn)**
  - Librería de utilidades para JavaScript
  - Utiliza los mejores métodos de librerias extendidas como Lodash o Underscore
  - Añade estos métodos a nuestro JavaScript mediante prototype

- **[Spotymix](https://github.com/OSWeekends/spoty-mix)**
  - Permite crear nuevas playlist
  - Fusiona canciones de diversas playlist
  - Permite juntar tus mejores canciones con las mejores canciones de otro amigo
  - Social Login integrado

- **[JSDayES golosinas IOT](https://github.com/UlisesGascon/JSDayES-golosinas-IOT)**
  - Orientado a IOT
  - Comunicación Serial
  - Gestión de dispositivos externos
  - No necesita HTTP

- **[Raspi  - System Info to Firebase](https://github.com/UlisesGascon/raspberrypi-system-info-data-to-firebase)**
  - Partiendo de otro repositorio/proyecto.
  - Monitorización del sistema
  - Uso de comandos de terminal
  - Gestión de procesos inestables
  - Integración con soluciones No-backend
  - Tiempo Real
  - No necesita HTTP

- **[IT Pulse](https://github.com/UlisesGascon/twitter-sentiments)**
  - Partiendo de otro repositorio/proyecto.
  - APIs de terceros
  - Stream directo de datos
  - Servidor Http
  - Tiempo Real y sincronía con WebSockets
  - Eventos
  - Evaluación semántica de la información
  - Sin Bases de datos

- **[MovieFire](https://github.com/UlisesGascon/Simple-API-REST-with-Firebase-and-IMBD)**
  - Integración con soluciones No-Backend
  - FrontEnd con Jade
  - BackEnd Flexible y dinámico
  - APIRest Cliente  -> Servidor
  - BackEnd con Express
  - CORS y Ajax

- **[AireMadrid](https://github.com/UlisesGascon/Aire-Madrid)**
  - Arquitectura alternativa en versiones anteriores
  - Conversión y parseo a Json
  - Procesamiento de datos en bruto
  - APIRest
  - Operaciones cíclicas gestionadas por Pillarsjs
  - FrontEnd con Jade
  - BackEnd con Express
  - Documentación con JSDocs

- **[Curratelo](https://github.com/UlisesGascon/curratelo)**
  - APIs de terceros
  - Stream directo de datos
  - Servidor Http
  - Tiempo Real y sincronía con WebSockets
  - Automatización con Slack y Hangouts


- **[Calidad del Aire con Firebase](https://github.com/UlisesGascon/Calidad-del-Aire-con-Firebase)**
  - Manejo de comunicación serial
  - Eventos y asincronía
  - IoT
