# Clase 11

### Expresiones Regulares

![Regular Expression XKCD](http://imgs.xkcd.com/comics/regular_expressions.png "RegEx save the day")
<sup>http://xkcd.com/208/</sup>


![email regex](http://i.imgur.com/7rV4c56.jpg "email regex")



**Creación**
```javascript
var expresionRegular = /fictizia/;
var expresionRegular = new RegExp("fictizia");
```

**[Trabajando con Flags](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/flags)**

- Flags disponibles:
    - [g](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/global), *Buscar en todo el texto*
    - [i](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/ignoreCase), *Case-insensitive*
    - [m](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/multiline), *Multilineal*
    - [y](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/sticky), *Sticky, recordando la última búsqueda*

- Usando Flags:
```javascript
var expresionRegular = /fictizia/gi;
var expresionRegular = new RegExp("fictizia", "gi");
```

**Métodos**
- [exec](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/RegExp/exec)
    Método RegExp que devuelve un array de información.
    ```javascript
    var coincidencias = /Fictizia/.exec('Hola desde Fictizia! Que te cuentas?');
    var coincidencias2 = /dato/.exec('Hola desde Fictizia! Que te cuentas?');
    console.log(coincidencias[0]); // Fictizia
    console.log(coincidencias2);   // null
    ```

- [test](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/RegExp/test)
    Método RegExp que verifica una coincidencia en una cadena. Devuelve true o false.
    ```javascript
    var coincidencias = /Fictizia/.test('Hola desde Fictizia! Que te cuentas?');
    var coincidencias2 = /dato/.test('Hola desde Fictizia! Que te cuentas?');
    console.log(coincidencias);  // true
    console.log(coincidencias2); // false
    ```

- [match](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/String/match)
    Método String que retorna un array de información o null si no existe coincidencia alguna.
    ```javascript
    var cadena = 'Hola desde Fictizia! Que te cuentas?';
    var erFictizia = /Fictizia/;
    var erDato = /dato/;
    var coincidencias = cadena.match(erFictizia);
    var coincidencias2 = cadena.match(erDato)
    console.log(coincidencias[0]); // Fictizia
    console.log(coincidencias2);   // null
    ```

- [search](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/String/search)
    Método String que devuelve el índice de la coincidencia, o -1 si la búsqueda falla.
    ```javascript
    var cadena = 'Hola desde Fictizia! Que te cuentas?';
    var erFictizia = /Fictizia/;
    var erDato = /dato/;
    var coincidencias = cadena.search(erFictizia);
    var coincidencias2 = cadena.search(erDato)
    console.log(coincidencias);   // 11
    console.log(coincidencias2);  // -1
    ```

- [replace](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/String/replace)
    Método String que reemplaza la subcadena encontrada con una subcadena de reemplazo.
    ```javascript
    var cadena = 'Hola desde Fictizia! Que te cuentas?';
    var erFictizia = /Fictizia/;
    var erDato = /dato/;
    var coincidencias = cadena.replace(erFictizia, "Cambiazo");
    var coincidencias2 = cadena.replace(erDato, "Cambiazo")
    console.log(coincidencias);   // Hola desde Cambiazo! Que te cuentas?
    console.log(coincidencias2);  // Hola desde Fictizia! Que te cuentas?
    ```

- [split](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/String/split)
    Un método String que retorna un array de subcadenas partiendo de un patrón dado.
    ```javascript
    var cadena = 'Hola desde Fictizia! Que te cuentas?';
    var erFictizia = /Fictizia/;
    var erDato = /dato/;
    var coincidencias = cadena.split(erFictizia);
    var coincidencias2 = cadena.split(erDato)
    console.log(coincidencias);   // ["Hola desde ", "! Que te cuentas?"]
    console.log(coincidencias2);  // ["Hola desde Fictizia! Que te cuentas?"]
    ```

###  Regex: Patrones complejos

Se utilizan los caracteres a, b, c y d para ilustrar los ejemplos.

**Básicos**

- ab
    Cualquier caracter o conjunto de caracteres. La busqueda se realiza de forma literal
    ```javascript
    //Encuentra solo el Lunes
    /Lunes/.test("Lunes")
    ```

**Anclajes**

- ^ab
    Coincide con el principio de la cadena o línea.
    ```javascript
    /^000/g.test("0001 es un id básico");
    ```

- ab$
    Coincide con el final de la cadena o línea.
    ```javascript
    /on$/.test("cancion")
    ```

**Metacaracteres**

Deberían ser escapados.

- .
    El metacaracter punto que coincide con cualquier carácter excepto *\n \r \u2028* o *\u2029*.
    ```javascript
    "That's hot!".match(/h.t/g);
    // [hat, hot]
    ```

- a|b
    Coincide con uno o con otro.
    ```javascript
    //Encuentra Lunes o Martes pero no Miercoles, jueves...
    /Lunes|Martes/.test("Martes")
    ```

- \
    Escapa un carácter específico.
    ```javascript
    /\*/.test("*")
    ```

**Cuantificadores**

- *
    Coincide con cero o más ocurrencias de la subexpresión que le precede al asterisco.
    ```javascript
    //Encuentra A, Ahhh, Ah, etc...
    /Ah*/.test("Ahhhhhhhh!")
    ```

- +
    El metacaracter suma coincide con una o más ocurrencias de la subexpresión que lo precede.
    ```javascript
    //Encuentra Ahhh, Ah, etc... pero no A.
    /Ah+/.test("Ahhhhhhhh!")
    ```

- ?
    Coincide con cero o un caracter.
    ```javascript
    //Encuentra A, Ahhh, An, Al, etc...
    /Ah?/.test("Ahhhhhhhh!")
    ```

- {X,y}
    Coincide un número determinado de veces.
    ```javascript
    // {2} Exactamente 2. Encuentra 11, 99, pero no 9, 987, etc...
    /[1-9]{2}/.test(12);

    // {2, 5} Exactametne entre 2 y 5. Encuentra 11, 666, 74511 pero no 1, 123456, etc...
    /[1-9]{2, 5}/.test(12345);

    // {2,} Exactamente 2 o más. Encuentra 11, 666, 74511 pero no 1,
    /[1-9]{2}/.test(123);
    ```



**Clases de caracteres**

- \d
    numérico (incluyendo _)
    ```javascript
    "Hola u_123! *.*".match(/\d/g);
    //["1", "2", "3"]
    ```

- \D
    No-numérico (incluyendo _)
    ```javascript
    "Hola u_123! *.*".match(/\D/g);
    // ["H", "o", "l", "a", " ", "u", "_", "!", " ", "*", ".", "*"]
    ```

- \s
    Busca un espacios en blanco
    ```javascript
    "Hola u_123! *.*".match(/\s/g);
    // [" ", " "]
    ```

- \S
    Busca que no coincida con un espacios en blanco
    ```javascript
    "Hola u_123! *.*".match(/\S/g);
    // ["H", "o", "l", "a", "u", "_", "1", "2", "3", "!", "*", ".", "*"]
    ```

- \w
    Busca caracteres alfanuméricos (_ incluido)
    ```javascript
    "Hola u_123! *.*".match(/\w/g);
    // ["H", "o", "l", "a", "u", "_", "1", "2", "3"]
    ```

- \W
    Busca que no coincida con un caracteres alfanuméricos (_ incluido)
    ```javascript
    "Hola u_123! *.*".match(/\W/g);
    // [" ", "!", " ", "*", ".", "*"]
    ```

- \b
    Busca la coincidencia al principio o fin de la palabra.
    ```javascript
    /\bme/g.test("menos")
    ```

- \B
    Busca la coincidencia evitando el principio o fin de la palabra.
    ```javascript
    /\Bte/g.test("bateria")
    ```

- \n
    Busca el salto de línea
    ```javascript
    /\n/.test("Hola!\nHola de nuevo...")
    ```

- \r
    Busca el retorno de carro
    ```javascript
    /\r/.test("Hola!\rHola de nuevo...")
    ```

- \t
    Busca la tabulación
    ```javascript
    /\t/.test("Hola!\tHola de nuevo...")
    ```


- \xxx
    Busca un caracter especificando el octal
    ```javascript
    // W -> 127
    /\127/g.test("Me gusta la Web")
    ```

- \uxxxx
    Busca un caracter unicode especificando en hexadecimal.
    ```javascript
    // W -> u0057
    /\u0057/g.test("Me gusta la Web")
    ```


**Agrupadores y Rangos**


- [ab]
    Coincide con al menos uno de los caracteres.
    ```javascript
    "hola... y de nuevo hola".match(/[hol]/g)
    // ["h", "o", "l", "o", "h", "o", "l"]
    ```

- [1-9]
    Rango entre 1 y 9
    ```javascript
    "172635172312312352451234...".match(/[1-4]/g)
    // ["1", "2", "3", "1", "2", "3", "1", "2", "3", "1", "2", "3", "2", "4", "1", "2", "3", "4"]
    ```

- [a-f]
    Rango alfabetico entre a y f.
    ```javascript
    "am3s5bdnd,ABCvm2naw8perjascm<lcmqPWD...".match(/[5-8q-zA-C]/g)
    // ["s", "5", "A", "B", "C", "v", "w", "8", "r", "s", "q"]
    ```

- [^ab]
    No debe coincidir con ningun de estos carácteres.
    ```javascript
    "hola... y de..".match(/[^hol]/g)
    // ["a", ".", ".", ".", " ", "y", " ", "d", "e", ".", "."]
    ```

- (ab)
    Agrupadores, permite crear un grupo
    ```javascript
    "Hola _quien seas_".replace(/_(.*?)_/, "<strong>$1</strong>")
    ```

- (?:ab)
    Grupo no capturado
    ```javascript
    "foo".match(/(foo){1,2}/) // ["foo", "foo"]
    "foo".match(/(?:foo){1,2}/) //["foo"]
    ```

- a(?=b)
    Encuentra *a* solo si *a* es seguido de *b*.
    ```javascript
    //Encuentra Metacaracter o Metacaracteres pero no Metadato.
    /Meta(?=caracter|caracteres)/.test("Metacaracter");
    ```

- a(?!y)
    Encuentra *a* solo si *a* no va seguido de *b*.
    ```javascript
    // Encuentra 141 pero no 3.141.
    /\d+(?!\.)/.test("3.141")
    ```


### Ejercicios

**1 -** Captura los emails del siguiente texto.
```
demo@demo.com, demo_demo@demo.com.ar, demo-demo12312@sub.dom.com.ar, demo@novalido, novalido>@demo.com, demo@novalido-.com, demo@-novalido.com
```

```javascript
var email = /[a-z0-9!#$%&'*+\/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+\/=?^_`{|}~-]+)*@(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?/g;
var emails = "demo@demo.com, demo_demo@demo.com.ar, demo-demo12312@sub.dom.com.ar, ,demo@novalido, novalido>@demo.com, demo@novalido-.com, demo@-novalido.com".match(email);
console.log(emails);
// ["demo@demo.com", "demo_demo@demo.com.ar", "demo-demo12312@sub.dom.com.ar"]
```


**2 -** Captura el DNI y NIE
- Formato DNI: 11223344-A (Guión opcional).

```
Válidos: 12345678-A, 11223344A,
No válidos: A11223344, 1234567K
```

- Formato para el NIE: X-1223344-A (Guión opcional).
    - El inicio puede ser X, Y o Z.

```
Válidos: X-1234567-A, X1234567A, Z1234567M
No válidos: X-1233456, 1234567
```

```javascript
var NIE = /([X-Z]{1})([-]?)(\d{7})([-]?)([A-Z]{1})/g;
var DNI = /(\d{8})([-]?)([A-Z]{1})/g;
var todosRegex = /(([X-Z]{1})([-]?)(\d{7})([-]?)([A-Z]{1}))|((\d{8})([-]?)([A-Z]{1}))/g;

var dniText = "Válidos: 12345678-A, 11223344A. No válidos: A11223344, 1234567K."
var nieText = "Válidos: X-1234567-A, X1234567A, Z1234567M. No válidos: X-1233456, 1234567."
var todosTexto = dniText + nieText

var DNIs = dniText.match(DNI);
var NIEs = nieText.match(NIE);
var todos = todosTexto.match(todosRegex)

console.log("Solo DNIs:", DNIs); // ["12345678-A", "11223344A"]
console.log("Solo NIEs:", NIEs); // ["X-1234567-A", "X1234567A", "Z1234567M"]
console.log("TODOS:", todos); // ["12345678-A", "11223344A", "X-1234567-A", "X1234567A", "Z1234567M"]
```

**3 -** Comprobar la seguridad de una contraseña

De esta forma comprobaremos:
- Contraseñas que contengan al menos una letra mayúscula.
- Contraseñas que contengan al menos una letra minúscula.
- Contraseñas que contengan al menos un número
- Contraseñas que contengan al menos un caracter especial @#$%.
- Contraseñas cuya longitud sea como mínimo 6 caracteres.
- Contraseñas cuya longitud máxima sea 20 caracteres.

```javascript
var test = `Válidas:
Z%C2Uacgw_4weL@Q
QZ6UttU-&r4t%R+J
KK8a%K^9seQ$Qc8X
*Q#*9-CP%?JkXQSs
#1234abCD@

No válidas:
?mT6JmKpTu6m_=g=
=G4T!v-J2_6aS^EW
perrito
perrito123
Perrito1234`;

var exp = /((?=.*\d)(?=.*[a-z])(?=.*[A-Z])(?=.*[@#$%]).{6,20})/gm;
var validas = test.match(exp);
// ["Z%C2Uacgw_4weL@Q", "QZ6UttU-&r4t%R+J", "KK8a%K^9seQ$Qc8X", "*Q#*9-CP%?JkXQSs", "#1234abCD@"]
```


### Versionado semantico

- [SemVer](http://semver.org/lang/es/)

### Código Limpio

- [Clean Code concepts adapted for JavaScript](https://github.com/ryanmcdermott/clean-code-javascript)

### Guías de estilo

- [Idiomatic.js](https://github.com/rwaldron/idiomatic.js/)
- [JavaScript Standard Style ](http://standardjs.com/#why-should-i-use-javascript-standard-style)
- [NPM](https://docs.npmjs.com/misc/coding-style)
- [Felix Geisendörfer](https://github.com/felixge/node-style-guide)
- [Airbnb](https://github.com/airbnb/javascript)
- [Google](https://google.github.io/styleguide/javascriptguide.xml)

### Documentation

- [JSDoc](http://usejsdoc.org/)

### Recursos

- [JS The Right Way](http://jstherightway.org/)
- [JavaScript Clean Coding Best Practices - Node.js at Scale](https://blog.risingstack.com/javascript-clean-coding-best-practices-node-js-at-scale/)
- [Clean Code JavaScript in CSS-TRICKS](https://css-tricks.com/clean-code-javascript/)
- [JavaScript Best Practices](https://www.devbridge.com/articles/javascript-best-practices/)
- [Untangling Spaghetti Code: How to Write Maintainable JavaScript](https://www.sitepoint.com/write-maintainable-javascript/)
- [Uncle Bob’s Clean Code: Irrelevant in the Age of Full-Stack JavaScript?](https://spin.atomicobject.com/2016/12/21/clean-code-full-stack-javascript/)

### ECMA6

- **Soporte**
	- [Compatibilidad de kangax](https://kangax.github.io/compat-table/es6/)

- **Compiladores y transpiladores**
	- [Babel - web](https://babeljs.io/)
	- [Babel - Github](https://babeljs.io/repl/)
	- [Lebab by David Walsh](https://davidwalsh.name/lebab)
	- [lebab - web](https://lebab.io/try-it)
	- [Lebab - Github](https://github.com/lebab/lebab)

- **Recursos**
	- [Pensar asíncronamente en un mundo síncrono](https://medium.com/@ulisesGascon/pensar-as%C3%ADncronamente-en-un-mundo-s%C3%ADncrono-8e25cfcafd83)
	- [ECMAScript 6 Features - jsfeatures.in](https://jsfeatures.in/ES6)
	- [ECMAScript 6 Features by Luke Hoban](https://github.com/lukehoban/es6features#readme)
	- [Learn ES2015 - A detailed overview of ECMAScript 6 features by Babel team](https://babeljs.io/docs/learn-es2015/)
	- [ECMAScript 6 Cheatsheet by Erik Moeller](http://help.wtf/es6)
	- [First Steps with ECMAScript 6 by Axel Rauschmayer](http://exploringjs.com/es6/ch_first-steps.html)
	- [JS Features by Hemanth.HM](http://jsfeatures.in/)
	- [Minimalist examples of ES6 functionalities by Hemanth.HM](https://github.com/hemanth/paws-on-es6)
	- [Understanding ECMAScript 6 by Nicholas C. Zakas](https://leanpub.com/understandinges6/read/)
	- [ES6 Overview in 350 Bullet Points by Ponyfoo](https://ponyfoo.com/articles/es6)
	- [Promise visualization playground for the adventurous](https://bevacqua.github.io/promisees/)
	- [ECMAScript 6 equivalents in ES5 by Addi Osmani](https://github.com/addyosmani/es6-equivalents-in-es5)


**[Principales cambios](http://es6-features.org/)**

- Constantes (cons):
```javascript
	const PI = 3.141593
  
  PI = 3.1; // Uncaught TypeError: Assignment to constant variable.
  
  const objeto = {
    usuario: "yo mismo",
    role: "profe"
  }
  
  objeto.role = "estudiante" // Propiedades no protegidas al cambio
  objeto.nuevo = "" // Se peuden crear nuevas propiedades
  objeto = "" // Uncaught TypeError: Assignment to constant variable. 
  
```

- Scoping:
	- Variables Internas (let):
	```javascript
		for (let i = 0; i < a.length; i++) {
	    let = a[i];
	    //...
		}

		/* ECMA5
		for (var i = 0; i < a.length; i++) {
	    var = a[i];
	    //...
		}
		*/
    
    
    var uno = 1;
    let dos = 2;
    if( uno === 1 ){
      var uno = 10;
      let dos = 20;
      console.log(uno); // 10
      console.log(dos); // 20
    }
    console.log(uno); // 10
    console.log(dos); // 2
    
	```
	- Funciones Internas:
	```javascript
		{
		    function nivel1 () { return 1 }
		    nivel1 ();
		    {
		        function nivel2() { return 2 }
		        nivel2 ();
		    }
		}

		/* ECMA5
		(function () {
		    var nivel1 = function () { return 1; }
		    nivel1();
		    (function () {
		        var nivel2 = function () { return 2; };
		        nivel2();
		    })();
		})();
		*/
	```

- Arrow Functions:
	- No pueden usarse con *yield*
	- No pueden ser usadas como constructores
	```javascript
	var Foo = () => {};
	var foo = new Foo(); // TypeError: Foo is not a constructor
	```
	- No tienen una propiedad de prototipo prototype
	```javascript
	var Foo = () => {};
	console.log(Foo.prototype); // undefined
	```
	- No pueden tener saltos de línea
	```javascript
	var func = ()
           => 1; 
	// SyntaxError: expected expression, got '=>'
	```
	- Retorno de objetos literales
	```javascript
	var func = () => {  foo: 1  };               
	// Al llamar func() retorna undefined!
	
	var func = () => {  foo: function() {}  };   
	// Error de sintaxis: SyntaxError: function statement requires a name
	
	// Funciona correctamente
	var func = () => ({ foo: 1 });
	```
	- Orden de parseo
	```javascript
	let callback;

	callback = callback || function() {}; // ok
	
	callback = callback || () => {};      
	// SyntaxError: invalid arrow-function arguments
	
	callback = callback || (() => {});    // ok
	```
	- Siempre son anónimas:
	```javascript
		impares  = numeros.map(v => v + 1);
		pares = evens.map(v => ({ even: v, odd: v + 1 }))
		otrosNumeros  = evens.map((v, i) => v + i)

		/* ECMA5
		impares  = numeros.map(function (v) { return v + 1; });
		pares = evens.map(function (v) { return { even: v, odd: v + 1 }; });
		otrosNumeros  = numeros.map(function (v, i) { return v + i; });
		*/

	```
	- *return* implicito en declaración inline
	```javascript
		var odds = [1,2,3,4,5].filter(num => num % 2);
		console.log(odds); // Array [ 1, 3, 5 ]
	```
	- *return* con cuerpo de bloque
	```javascript
	var func = x => x * x;                  
	// sintaxis de cuerpo conciso, el "return" está implícito
	
	var func = (x, y) => { return x + y; }; 
	// con cuerpo de bloque, se necesita "return" explícito
	```
	- *this* contextual:
	```javascript
	this.nums.forEach((v) => {
	    if (v % 5 === 0)
	        this.fives.push(v)
	})

	/* ECMA 5
	var self = this;
	this.nums.forEach(function (v) {
	    if (v % 5 === 0)
	        self.fives.push(v);
	});
	*/
	```
	- Las Arrow functions no exponen un objeto arguments 
	```javascript
	var arguments = 42;
	var arr = () => arguments;
	
	arr(); // 42
	
	function foo() {
	  var f = () => arguments[0]; // Referencia al objeto arguments
	  return f(2);
	}
	
	foo(1); // 1
	```
	- El parámetro rest es la mejor alternativa
	```javascript
	function foo() { 
	  var f = (...args) => args[0]; 
	  return f(2); 
	}
	
	foo(1); // 2
	```
	- Arrow functions usadas como métodos
	```javascript
	var obj = {
	  i: 10,
	  b: () => console.log(this.i, this),
	  c: function() {
	    console.log(this.i, this);
	  }
	}
	obj.b(); // prints undefined, Window {...} (or the global object)
	obj.c(); // prints 10, Object {...}
	```
	```javascript
	var obj = {
	  a: 10
	};
	
	Object.defineProperty(obj, 'b', {
	  get: () => {
	    console.log(this.a, typeof this.a, this);
	    return this.a + 10; // represents global object 'Window', therefore 'this.a' returns 'undefined'
	  }
	});
	```
	- Invocación a través de los métodos call y apply
	```javascript
	var adder = {
	  base : 1,
	    
	  add : function(a) {
	    var f = v => v + this.base;
	    return f(a);
	  },
	
	  addThruCall: function(a) {
	    var f = v => v + this.base;
	    var b = {
	      base : 2
	    };
	            
	    return f.call(b, a);
	  }
	};
	
	console.log(adder.add(1));         // Imprime 2 como es esperado
	console.log(adder.addThruCall(1)); // También imprime 2 aunque se esperaba 3
	```
	- Sintaxis básica
	```javascript
	(param1, param2, paramN) => {declaraciones} 
	(param1, param2, paramN) =>expresion
	// Equivalente a: () => { return expresion; } 
	
	// Los paréntesis son opcionales cuando sólo dispone de un argumento: singleParam => { statements } 
	singleParam => expresion 
	
	// Una función sin argumentos requiere paréntesis: 
	() => { declaraciones }
	```
	- Sintaxis Avanzada
	```javascript
	// Incluir entre paréntesis el cuerpo para retornar un objeto literal:
	params => ({foo: bar})
	
	// Soporta parámetros rest y parámetros por default
	(param1, param2, ...rest) => { statements }
	(param1 = valorPredef1, param2, ..., paramN = valorPredefN) => { statements }
	
	// Destructuración mediante la lista de parámetros también es soportada
	var f = ([a, b] = [1, 2], {x: c} = {x: a + b}) => a + b + c; f(); // 6
	```

- Gestión de Parámetros en funciones:
	- Parametros opcionales:
	```javascript
		function f (x, y = 7, z = 42) {
		    return x + y + z
		}

		/* ECMA5
		function f (x, y, z) {
		    if (y === undefined){
				y = 7;
			}
		    z = z || 42;
		    return x + y + z;
		};
		*/
	```
	- Parametros adicionales:
	```javascript
		function f (x, y, ...a) {
		    return (x + y) * a.length
		}

		/* ECMA5
		function f (x, y) {
		    var a = Array.prototype.slice.call(arguments, 2);
		    return (x + y) * a.length;
		};
		*/
	```

- Las plantillas de cadena de texto:
	- Concepto:
	```javascript
		`cadena de texto ${expresión} texto`
	```
	- Multiples líneas:
	```javascript
		console.log(`línea 1 de texto
		línea 2 de texto`);

		/* ECMA5
		console.log("línea 1 de texto\nlínea 2 de texto");
		*/
	```
	- Expresiones:
	```javascript
		var customer = { name: "Foo" }
		var card = { amount: 7, product: "Bar", unitprice: 42 }
		message = `Hello ${customer.name},
		want to buy ${card.amount} ${card.product} for
		a total of ${card.amount * card.unitprice} bucks?`

		/* ECMA5
		var customer = { name: "Foo" };
		var card = { amount: 7, product: "Bar", unitprice: 42 };
		message = "Hello " + customer.name + ",\n" +
		"want to buy " + card.amount + " " + card.product + " for\n" +
		"a total of " + (card.amount * card.unitprice) + " bucks?";
		*/
	```
- Mejoras en Objetos (propiedades y métodos):
	- Acortador de propiedades
	```javascript
	let obj = { x, y }
	
	/* ECMA5
	var obj = { x: x, y: y };
	*/
	```
	- Definición de propiedades computerizadas:
	```javascript
		obj = {
		    foo: "bar",
		    [ "prop_" + foo() ]: 42
		}

		/* ECMA5
		obj = {
		    foo: "bar"
		};
		obj[ "prop_" + foo() ] = 42;
		*/
	```
	- Métodos:
	```javascript
		obj = {
		    foo (a, b) {
		        …
		    },
		    bar (x, y) {
		        …
		    },
		    // Generador
		    *quux (x, y) {
		        …
		    }
		}

		/* ECMA5
		obj = {
		    foo: function (a, b) {
		        …
		    },
		    bar: function (x, y) {
		        …
		    },
		    //  quux: no equivalent in ES5
		    …
		};
		*/
	```
- Parsear Binarios y Octales:
```javascript
	0b111110111 === 503
	0o767 === 503

	/* ECMA 5
	parseInt("111110111", 2) === 503;
	parseInt("767", 8) === 503;
	*/
```

- Asignación desestructurada:
	- Objetos:
	```javascript
	//Object Matching, Shorthand Notation & Deep Matching
	var { op: a, lhs: { op: b }, rhs: c } = getASTNode()
	
	//Default Values
	var obj = { a: 1 }
	var { a, b = 2 } = obj
	
	// Parameter Context Matching
	function g ({ name: n, val: v }) {
    	console.log(n, v)
	}
	function h ({ name, val }) {
	    console.log(name, val)
	}
	g({ name: "foo", val:  7 })
	h({ name: "bar", val: 42 })
	
	/* ECMA5
	//Object Matching, Shorthand Notation & Deep Matching
	var tmp = getASTNode();
	var a = tmp.op;
	var b = tmp.lhs.op;
	var c = tmp.rhs;
	
	//Default Values
	var obj = { a: 1 };
	var a = obj.a;
	var b = obj.b === undefined ? 2 : obj.b;
	
	// Parameter Context Matching
	function g (arg) {
	    var n = arg.name;
	    var v = arg.val;
	    console.log(n, v);
	};
	function h (arg) {
	    var name = arg.name;
	    var val  = arg.val;
	    console.log(name, val);
	
	g({ name: "foo", val:  7 });
	h({ name: "bar", val: 42 });
	*/
	```
	- Arrays:
	```javascript
		// Matching
		var list = [ 1, 2, 3 ]
		var [ a, , b ] = list

		// Parameter Context Matching
		function f ([ name, val ]) {
		    console.log(name, val)
		}

		f([ "bar", 42 ]);

		// Fail-Soft Destructuring
		var list2 = [ 7, 42 ]
		var [ a = 1, b = 2, c = 3, d ] = list2

		/* ECMA5
		// Matching
		var list = [ 1, 2, 3 ];
		var a = list[0], b = list[2];

		// Parameter Context Matching
		function f (arg) {
		    var name = arg[0];
		    var val  = arg[1];
		    console.log(name, val);
		};

		f([ "bar", 42 ]);

		// Fail-Soft Destructuring
		var list2 = [ 7, 42 ];
		var a = typeof list2[0] || 1;
		var b = typeof list2[1] || 2;
		var c = typeof list2[2] !== "undefined" ? list2[2] : 3;
		var d = typeof list2[3] !== "undefined" ? list2[3] : undefined;
		*/
	```
- Nuevos Métodos Integrados:
	- Asignación de propiedades enumerables en objetos:
	```javascript
		var dst  = { quux: 0 }
		var src1 = { foo: 1, bar: 2 }
		var src2 = { foo: 3, baz: 4 }
		Object.assign(dst, src1, src2)

		// Verificación
		dst.quux === 0
		dst.foo  === 3
		dst.bar  === 2
		dst.baz  === 4

		/* ECMA5
		var dst  = { quux: 0 };
		var src1 = { foo: 1, bar: 2 };
		var src2 = { foo: 3, baz: 4 };
		Object.keys(src1).forEach(function(k) {
		    dst[k] = src1[k];
		});
		Object.keys(src2).forEach(function(e) {
		    dst[k] = src2[k];
		});

		// Verificación
		dst.quux === 0;
		dst.foo  === 3;
		dst.bar  === 2;
		dst.baz  === 4;
		*/
	```
	- Repetir
	```javascript
	" ".repeat(4 * depth)
	"foo".repeat(3)
	
	/* ECMA5
	Array((4 * depth) + 1).join(" ");
	Array(3 + 1).join("foo");
	*/
	```
	- Busqueda en sub-cadenas:
	```javascript
		"hello".startsWith("ello", 1) // true
		"hello".endsWith("hell", 4)   // true
		"hello".includes("ell")       // true
		"hello".includes("ell", 1)    // true
		"hello".includes("ell", 2)    // false

		/* ECMA5
		"hello".indexOf("ello") === 1;    // true
		"hello".indexOf("hell") === (4 - "hell".length); // true
		"hello".indexOf("ell") !== -1;    // true
		"hello".indexOf("ell", 1) !== -1; // true
		"hello".indexOf("ell", 2) !== -1; // false
		*/
	```
	- Chequear No-Numericos e infinitos:
	```javascript
		Number.isNaN(42) === false
		Number.isNaN(NaN) === true

		Number.isFinite(Infinity) === false
		Number.isFinite(-Infinity) === false
		Number.isFinite(NaN) === false
		Number.isFinite(123) === true

		/* ECMA5
		var isNaN = function (n) {
		    return n !== n;
		};
		var isFinite = function (v) {
		    return (typeof v === "number" && !isNaN(v) && v !== Infinity && v !== -Infinity);
		};
		isNaN(42) === false;
		isNaN(NaN) === true;

		isFinite(Infinity) === false;
		isFinite(-Infinity) === false;
		isFinite(NaN) === false;
		isFinite(123) === true;
		*/
	```
	- isSafeInteger():
	```javascript
		Number.isSafeInteger(42) === true
		Number.isSafeInteger(9007199254740992) === false

		/* ECMA5
		function isSafeInteger (n) {
		    return (
		           typeof n === 'number'
		        && Math.round(n) === n
		        && -(Math.pow(2, 53) - 1) <= n
		        && n <= (Math.pow(2, 53) - 1)
		    );
		}
		isSafeInteger(42) === true;
		isSafeInteger(9007199254740992) === false;
		*/
	```
	- Truncar Número Flotante:
	```javascript
	console.log(Math.trunc(42.7)) // 42
	console.log(Math.trunc( 0.1)) // 0
	console.log(Math.trunc(-0.1)) // -0

	/* ECMA5
	function mathTrunc (x) {
	    return (x < 0 ? Math.ceil(x) : Math.floor(x));
	}
	console.log(mathTrunc(42.7)) // 42
	console.log(mathTrunc( 0.1)) // 0
	console.log(mathTrunc(-0.1)) // -0
	*/
	```

- For... of (iteración sobre valores y no propiedades):
```javascript
  let arr = [3, 5, 7];
  arr.foo = "hello";

  for (let i in arr) {
     console.log(i);
     // "0", "1", "2", "foo"
  }

  for (let i of arr) {
     console.log(i);
     // "3", "5", "7"
  }

```
- Generadores:
	- [Ejemplo de Miguel Sánchez](http://miguelsr.js.org/2015/06/08/es6-generators.html)
	```javascript
		function* greatGenerator(name) {
		    yield "Hola " + name + "!";
		    yield "Esta línea saldrá en la segunda ejecución";
		    yield "Esta otra, en la tercera";
		    if (name === "Miguel") yield "Esta otra, saldrá en la cuarta solo si te llamas miguel"
		}
		var generatorInstance = greatGenerator("paco");
		console.log(generatorInstance.next().value); // Hola paco!
		console.log(generatorInstance.next().value); // Esta línea saldrá la segunda ejecución
		console.log(generatorInstance.next().value); // Esta otra, en la tercera
		console.log(generatorInstance.next().value); // undefined
	```

- Map:
	- Manejando datos independientes con una estructura clave/valor
	```javascript
		let miMap = new Map();
		let miArray = [];

		miMap.set('cadena', 'Hola!');
		miMap.set(miArray, [500, "hola", true, false]);

		console.log(miMap.get(miArray)); // [500, "hola", true, false]
		console.log(miMap.get('cadena')); // Hola!

		console.log(miMap.size); // 2

		miMap.delete('cadena');

		console.log(miMap.size); // 1
	```
- Set:
	```javascript
	let s = new Set()
	s.add("hello").add("goodbye").add("hello")
	s.size === 2
	s.has("hello") === true
	for (let key of s.values()) // insertion order
	    console.log(key)
	```
- Set vs Map:
	```javascript
	//@see: https://stackoverflow.com/a/24085746
	var array = [1, 2, 3, 3];

	var set = new Set(array); // Will have [1, 2, 3]
	assert(set.size, 3);
	
	var map = new Map();
	map.set('a', 1);
	map.set('b', 2);
	map.set('c', 3);
	map.set('C', 3);
	map.set('a', 4); // Has: a, 4; b, 2; c: 3, C: 3
	assert(map.size, 4);
	```

- Clases:
	- La idea es POO sin prototipos
	- Definición de Clase:
	```javascript
		class coche{
		  constructor(marca, modelo, antiguedad, color, tipo) {
		    this.marca = marca;
		    this.modelo = modelo;
		    this.antiguedad = antiguedad;
		    this.color = color;
		    this.tipo = tipo;
		  }
		  detalles() {
		    console.log(`Tu coche es un ${this.marca} ${this.modelo} con ${this.antiguedad} años, clase ${this.tipo} y color ${this.color}`);
		  }
		}

		let miCoche = new coche ("Seat", "Panda", 20, "azul", "turismo");
		miCoche.detalles();

		/* ECMA 5
		var coche = function (marca, modelo, antiguedad, color, tipo) {
		    // Propiedades
		    this.marca = marca;
		    this.modelo = modelo;
		    this.antiguedad = antiguedad;
		    this.color = color;
		    this.tipo = tipo;
		    // Metodos
		    this.detalles = function(){
		      console.log("Tu coche es un "+this.marca+" "+this.modelo+" con "+this.antiguedad+" años, clase "+this.tipo+" y color "+this.color);
		    }
		};

		var miCoche = new coche ("Seat", "Panda", 20, "azul", "turismo");
		miCoche.detalles();
		*/
	```
	- Extensión de Clase:
	```javascript
		class perro {
		  constructor(nombre) {
		    this.patas = 4;
		    this.ojos = 2;
		    this.nombre = nombre;
		  }

		  ladrar() {
		    console.log(`${this.nombre} esta ladrando!`);
		  };
		}

		class pastorAleman extends perro {
		  constructor(nombre) {
		    super('pastorAleman');
		    this.colorLengua = "negra";
		    this.colorOjos = "marrón";
		    this.capacidadTrabajo = true;
		    this.especialidad = "Pastoreo";
		  }

		  informacion() {
		  	console.log(`Nombre: ${this.nombre}
		  	Número patas: ${this.patas}
		  	Número ojos: ${this.ojos}
		  	Color ojos: ${this.colorOjos}
		  	Color Lengua: ${this.colorLengua}
		  	Capacidad de trabajo: ${this.capacidadTrabajo}
		  	Especialidad: ${this.especialidad}`);
		  }
		}

		let miPerro = new pastorAleman('Golden');
		miPerro.informacion();
		miPerro.ladrar();

		/* ECMA 5
		var perro  = function (nombre) {
		    this.patas = 4;
		    this.ojos = 2;
		    this.nombre = nombre;
		    this.ladrar = function(){
		    	console.log(this.nombre + " esta ladrando!");
		    }
		};

		var pastorAleman = function () {
		    this.colorLengua = "negra";
		    this.colorOjos = "marrón";
		    this.capacidadTrabajo = true;
		    this.especialidad = "Pastoreo";
		    this.informacion = function(){
				console.log("Nombre: "+this.nombre+"\nNúmero patas: "+this.patas+"\nNúmero ojos: "+this.ojos+"\nColor Lengua: "+this.colorLengua+"\nColor ojos: "+this.colorOjos+"\nCapacidad de trabajo: "+this.capacidadTrabajo+"\nEspecialidad: "+this.especialidad);
		    }
		};

		pastorAleman.prototype = new perro("Golden");

		var miPerro = new pastorAleman();
		miPerro.informacion();
		miPerro.ladrar();
		*/
	```
	- Métodos Estáticos:
	```javascript
		class coche{
		  static info (edad){
		  	console.info(`Tienes ${edad} años ${ edad >= 18 ? "y puedes conduccir": "y ... ¡Sorpresa! No puedes conduccir."}`);
		  }
		  constructor(marca, modelo, antiguedad, color, tipo) {
		    this.marca = marca;
		    this.modelo = modelo;
		    this.antiguedad = antiguedad;
		    this.color = color;
		    this.tipo = tipo;
		  }
		  detalles() {
		    console.log(`Tu coche es un ${this.marca} ${this.modelo} con ${this.antiguedad} años, clase ${this.tipo} y color ${this.color}`);
		  }
		}

		coche.info(50);
		coche.info(8);
		let miCoche = new coche ("Seat", "Panda", 20, "azul", "turismo");
		miCoche.detalles();
	```
	- Getter/Setter
	```javascript
	class Rectangle {
	    constructor (width, height) {
	        this._width  = width
	        this._height = height
	    }
	    set width  (width)  { this._width = width               }
	    get width  ()       { return this._width                }
	    set height (height) { this._height = height             }
	    get height ()       { return this._height               }
	    get area   ()       { return this._width * this._height }
	}
	var r = new Rectangle(50, 20)
	r.area === 1000
	
	/* ECMA5
	var Rectangle = function (width, height) {
	    this._width  = width;
	    this._height = height;
	};
	Rectangle.prototype = {
	    set width  (width)  { this._width = width;               },
	    get width  ()       { return this._width;                },
	    set height (height) { this._height = height;             },
	    get height ()       { return this._height;               },
	    get area   ()       { return this._width * this._height; }
	};
	var r = new Rectangle(50, 20);
	r.area === 1000;
	*/
	```
- Módulos (Exportación):
	- Único
	```javascript
		// config.js
		let config = {
			token: "secreto",
		}

		export default config;
	```
	- Mutiples
	```javascript
		// config.js
		let config = {
			token: "secreto",
		}

		let config_details = {
			detalles: "más datos"
		}

		export config;
		export config_details;
	```
	- Combinada
	```javascript
		// config.js
		let config = {
			token: "secreto",
		}

		let config_details = {
			detalles: "más datos"
		}

		let configuraciones = {config, config_details}

		export default configuraciones;
		export config;
		export config_details;
	```
- Módulos (Importación):
	- Síncrona
	```javascript
		// único
		import config from './config.js';

		// Multiples
		import * as config from './config.js';

		// Combinandos
		import configuraciones from './config.js';
		import { config, config_details } from './config.js';
	```
	- Asíncrona (solo un módulo)
	```javascript
		System.import('modulo')

	    .then(modulo => {
	        // Uso del módulo importado
	    })
	    .catch(error => {
	        // Gestión de errores
	    });
	```
	- Asíncrona (multiples módulos)
	```javascript
	    Promise.all(
	        ['module1', 'module2', 'module3']
	        .map(x => System.import(x)))
	    .then(([module1, module2, module3]) => {
	        // Use module1, module2, module3
	    });
	```
- Módulos (Comparativa):
	- Export/Import
	```javascript
	//  lib/math.js
	export function sum (x, y) { return x + y }
	export var pi = 3.141593
	
	//  someApp.js
	import * as math from "lib/math"
	console.log("2π = " + math.sum(math.pi, math.pi))
	
	//  otherApp.js
	import { sum, pi } from "lib/math"
	console.log("2π = " + sum(pi, pi))

	/* ECMA5
	//  lib/math.js
	LibMath = {};
	LibMath.sum = function (x, y) { return x + y };
	LibMath.pi = 3.141593;
	
	//  someApp.js
	var math = LibMath;
	console.log("2π = " + math.sum(math.pi, math.pi));
	
	//  otherApp.js
	var sum = LibMath.sum, pi = LibMath.pi;
	console.log("2π = " + sum(pi, pi));
	
	*/
	```
	- Default & Wildcard
	```javascript
	//  lib/mathplusplus.js
	export * from "lib/math"
	export var e = 2.71828182846
	export default (x) => Math.exp(x)
	
	//  someApp.js
	import exp, { pi, e } from "lib/mathplusplus"
	console.log("e^{π} = " + exp(pi))
	
	/* ECMA5
	//  lib/mathplusplus.js
	LibMathPP = {};
	for (symbol in LibMath)
	    if (LibMath.hasOwnProperty(symbol))
	        LibMathPP[symbol] = LibMath[symbol];
	LibMathPP.e = 2.71828182846;
	LibMathPP.exp = function (x) { return Math.exp(x) };
	
	//  someApp.js
	var exp = LibMathPP.exp, pi = LibMathPP.pi, e = LibMathPP.e;
	console.log("e^{π} = " + exp(pi));
	*/
	```
- Typed Arrays
	```javascript
	//  ES6 class equivalent to the following C structure:
	//  struct Example { unsigned long id; char username[16]; float amountDue }
	class Example {
	    constructor (buffer = new ArrayBuffer(24)) {
	        this.buffer = buffer
	    }
	    set buffer (buffer) {
	        this._buffer    = buffer
	        this._id        = new Uint32Array (this._buffer,  0,  1)
	        this._username  = new Uint8Array  (this._buffer,  4, 16)
	        this._amountDue = new Float32Array(this._buffer, 20,  1)
	    }
	    get buffer ()     { return this._buffer       }
	    set id (v)        { this._id[0] = v           }
	    get id ()         { return this._id[0]        }
	    set username (v)  { this._username[0] = v     }
	    get username ()   { return this._username[0]  }
	    set amountDue (v) { this._amountDue[0] = v    }
	    get amountDue ()  { return this._amountDue[0] }
	}
	
	let example = new Example()
	example.id = 7
	example.username = "John Doe"
	example.amountDue = 42.0
	```
- Promesas
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
- [Internationalization & Localization](http://kangax.github.io/compat-table/esintl/
)
- [Software design patterns implemented in EcmaScript 6](http://loredanacirstea.github.io/es6-design-patterns/)


## ES2016 (ES7)

### Recursos

- [Compatibilidad 2016+](http://kangax.github.io/compat-table/esnext/)
- [Axel Rauschmayer blog | ES2016](http://2ality.com/2016/01/ecmascript-2016.html)

### Implementados

#### [Array.prototype.includes](http://2ality.com/2016/02/array-prototype-includes.html)
- Retorna un booleno si encuentra el elemento. Tiene muchas mejoras respecto `Array.prototype.indexOf`
- Propuesto por Domenic Denicola y Rick Waldron
- [ECMA documentation](http://www.ecma-international.org/ecma-262/7.0/index.html#sec-array.prototype.includes)

```javascript
['a', 'b', 'c'].includes('a') // true
['a', 'b', 'c'].includes('d') // false

// similitud con indexOf:
// arr.includes(x) -> arr.indexOf(x) >= 0

[NaN].includes(NaN) // true
[NaN].indexOf(NaN) // -1

// @see: http://speakingjs.com/es5/ch11.html#two_zeros
[-0].includes(+0) // true

// Arrays tipados también lo incluyen
let arrTip = Uint8Array.of(12, 5, 3);
console.log(arrTip.includes(5)); // true
```


#### [Exponentiation Operator](http://2ality.com/2016/02/exponentiation-operator.html)
- Añade el operador exponencial `x ** y`, su comportamiento es igual que `Math.pow(x, y)`
- Propuesto por Rick Waldron, Claude Pache y Brendan Eich
- [ECMA documentation](http://www.ecma-international.org/ecma-262/7.0/index.html#sec-exp-operator)

```javascript
let cuadrado = 3 ** 2; // 9
let numero = 3;
numero **= 2; // 9
```

## ES2017 (ES8)

### Recursos
- [Async Functions for ECMAScript](https://github.com/tc39/ecmascript-asyncawait)
- [Simplifying asynchronous computations via generators (section in “Exploring ES6”)](http://exploringjs.com/es6/ch_generators.html#sec_co-library)
- [ES proposal: async functions](http://2ality.com/2016/02/async-functions.html)
- [Compatibilidad 2016+](http://kangax.github.io/compat-table/esnext/)
- [Axel Rauschmayer blog | ES2017](http://2ality.com/2016/02/ecmascript-2017.html)
- [Shared memory and atomics for ECMAscript](https://github.com/tc39/ecmascript_sharedmem)
- [ES8, el estándar Javascript de 2017 de Enrique Oriol](http://blog.enriqueoriol.com/2017/07/es8-estandar-javascript-2017.html)

### Implementados cambios principales

#### [Async Functions](http://2ality.com/2016/02/async-functions.html)
Propuesto por Brian Terlson


**Código asíncrono utilizando promesas y generadores**
- Nativo

```javascript
function fetchJson(url) {
    return fetch(url)
    .then(request => request.text())
    .then(text => {
        return JSON.parse(text);
    })
    .catch(error => {
        console.log(`ERROR: ${error.stack}`);
    });
}
fetchJson('http://example.com/some_file.json')
.then(obj => console.log(obj));
```

- Usando la librería [CO](https://github.com/tj/co)

```javascript
const fetchJson = co.wrap(function* (url) {
    try {
        let request = yield fetch(url);
        let text = yield request.text();
        return JSON.parse(text);
    }
    catch (error) {
        console.log(`ERROR: ${error.stack}`);
    }
});
```

- Funciones Asíncronas

```javascript
async function fetchJson(url) {
    try {
        let request = await fetch(url);
        let text = await request.text();
        return JSON.parse(text);
    }
    catch (error) {
        console.log(`ERROR: ${error.stack}`);
    }
}
```

- Variantes

```javascript
// Declaración
async function foo() {}

// Expresión
const foo = async function () {};

// Método
let obj = { async foo() {} }

// Formato Arrow Function
const foo = async () => {};
```

#### [Shared memory and atomics](http://2ality.com/2017/01/shared-array-buffer.html)
- Gestiona multithreading en Javascript, basicamente *web wrokers* con `ArrayBuffer`. Muy bajo nivel
- Propuesto por Lars T. Hansen

### Implementados cambios menores

#### [Object.values/Object.entries](http://2ality.com/2015/11/stage3-object-entries.html)
- Retornan un array o una matriz con los valores o las propiedades iterables de los objetos
- Propuesto por Jordan Harband

**Objetos**
```javascript
const data = { elemnto: 'primero', otro_dato: 1 };
Object.values(data); // ["primero", 1]
Object.entries(data); // [["elemnto", "primero"], ["otro_dato", 1]]
```

**Arrays**
```javascript
const obj = ['p1', 'p2', 'p3']; // equivale a { 0: 'a', 1: 'z', 2: '5' };
Object.values(obj); // ["p1", "p2", "p3"]
Object.entries(obj); // [["0", "p1"], ["1", "p2"], ["2", "p3"]]
```

**Cadenas**
```javascript
Object.values('Fictizia'); // ["F", "i", "c", "t", "i"]
Object.entries('Ficti'); // [["0", "F"], ["1", "i"], ["2", "c"], ["3", "t"], ["4", "i"]]
```


#### [String padding](http://2ality.com/2015/11/string-padding.html)
- Se rellena una cadena por delante con `padStart` o por detras con `padEnd`, puedes especificar el relleno
- Propuesto por Jordan Harband y Rick Waldron

```javascript
// .padStart()
'data'.padStart(1);          // "data"
'data'.padStart(7);          // "   data"
'data'.padStart(6, '-');     // "--data"
'data'.padStart(10, 'info'); // "infoindata"
'data'.padStart(5, '*');     // "*data"

// .padEnd()
'data'.padEnd(1);          // "data"
'data'.padEnd(7);          // "data   "
'data'.padEnd(6, '-');     // "data--"
'data'.padEnd(10, 'info'); // "datainfoin"
'data'.padEnd(5, '*');     // "data*"
```

#### [Object.getOwnPropertyDescriptors()](http://2ality.com/2016/02/object-getownpropertydescriptors.html)
- No confundir con `Object.getOwnPropertyDescriptor()`
- Devulve las descripciones de las propiedades del objeto
- Esta pensado para facilitar el clonado de objetos ya que no se muestran propiedades heredadas por prototype
- Propuesto por Jordan Harband y Andrea Giammarchi
- [Object.getOwnPropertyDescriptors Proposal ](https://github.com/tc39/proposal-object-getownpropertydescriptors)


#### [Trailing commas in function parameter lists and calls](http://2ality.com/2015/11/trailing-comma-parameters.html)
- Se ignorarán las comas extras al final del último parámetro de una función
- Propuesto por Jeff Morrison

```javascript
//En declaración
function foo(p1, p2, p3,) { 
    //... 
}

//En ejecución
foo("a", "b", "b",);
```

## ES2018 (ES9)

### Recursos
- [Compatibilidad ES Next](http://kangax.github.io/compat-table/esnext/)
- [Compatibilidad 2016+](http://kangax.github.io/compat-table/esnext/)
- [Axel Rauschmayer blog | ES2018](http://2ality.com/2017/02/ecmascript-2018.html?utm_source=hashnode.com)

### Candidatos muy probables (stage 4)

- [Template Literal Revision](http://2ality.com/2016/09/template-literal-revision.html) *Propuesto por Tim Disney*
- [s (dotAll) flag for regular expressions](http://2ality.com/2017/07/regexp-dotall-flag.html) *Propuesto por Mathias Bynens*

### Candidatos poco probables (stage 3)

- [Function.prototype.toString revision](http://2ality.com/2016/08/function-prototype-tostring.html) *Propuesto por Michael Ficarra*
- [global](http://2ality.com/2016/09/global.html) *Propuesto por Jordan Harband*
- [Rest/Spread Properties](http://2ality.com/2016/10/rest-spread-properties.html) *Propuesto por Sebastian Markbåge*
- [Asynchronous Iteration](http://2ality.com/2016/10/asynchronous-iteration.html) *Propuesto por Domenic Denicola*
- [import()](http://2ality.com/2017/01/import-operator.html) *Propuesto por Domenic Denicola*
- [RegExp Lookbehind Assertions](http://2ality.com/2017/05/regexp-lookbehind-assertions.html) *Propuesto por Daniel Ehrenberg*
- [RegExp Unicode Property Escapes](http://2ality.com/2017/07/regexp-unicode-property-escapes.html) *Propuesto por Brian Terlson, Daniel Ehrenberg y Mathias Bynens*
- [RegExp named capture groups](http://2ality.com/2017/05/regexp-named-capture-groups.html) *Propuesto por Daniel Ehrenberg y Brian Terlson*
- [Promise.prototype.finally()](http://2ality.com/2017/07/promise-prototype-finally.html) *Propuesto por Jordan Harband*
- [BigInt – arbitrary precision integers](http://2ality.com/2017/03/es-integer.html) *Propuesto por Daniel Ehrenberg*
- [Class fields](http://2ality.com/2017/07/class-fields.html) *Propuesto por Daniel Ehrenberg y Jeff Morrison*
- [Optional catch binding](http://2ality.com/2017/08/optional-catch-binding.html) *Propuesto por Michael Ficarra*
- [import.meta](http://2ality.com/2017/11/import-meta.html) *Propuesto por Domenic Denicola*
- [Array.prototype.flatMap/flatten](http://2ality.com/2017/04/flatmap.html) *Propuesto por Brian Terlson y Michael Ficarra*