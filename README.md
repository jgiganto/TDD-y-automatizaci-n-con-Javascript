TDD y automatización con Javascript
===================================

#Introducción:

 En procesos ágiles de desarrollo, iterativos y de ciclos cortos es muy valiso el feedback de los usuarios. Para esto necesitamos poder poner a disposición de los mismos versiones de la aplicación lo más rápido posible.
 Las pruebas unitarias (unit testing) nos permiten desarrollar piezas del sistema sin necesitar otras dependecias y además asegurar la calidad del código a medida que el sistema crece.
 Automatizar la ejecución de las pruebas unitarias nos permite detectar problemas de integración, por otro lado la automatización del proceso de despliegue nos ahorra el tiempo de hacerlo nosotros y evita errores humanos.
 En este artículo vamos a hacer una introducción a conceptos de unit testing, integración continua, despligue automatizado y entrega continua.
 Para ello utilizaremos Nodejs, gruntjs, Qunit, Phantomjs y Karma.
 La idea es una introcción general a los conceptos y herramienteas sin profundizar en particular.

#Pruebas unitarias
 
 Las pruebas unitarias son bien conocidas y nos permitirán probar piezas de nuestro código de manera aislada, por ejemplo:
 
```javascript
function suma(a, b) {
	return a + b;
}

function test() {
	var valorEsperado = 7;
	if (suma(3, 4) != valorEsperado)
		throw new Error();
}

test();
```

[Ejemplo 1](https://github.com/leomicheloni/TDD-y-automatizaci-n-con-Javascript/blob/master/examples/example_1.js)

Por supuesto que esto es un ejemplo, pero la idea básica es tener una porción de código que prueba nuestro código, como imaginarán para esto exiten muchos frameworks:

<ul>
<li>[QUnit](http://qunitjs.com/)</li>
<li>[Jasmine](http://jasmine.github.io/)</li>
<li>[JUnit](http://junit.org/)</li>
</ul>

Y tantos más, en este caso vamos a utilizar QUnit.

##Introducción a QUnit
Básicamente tenemos una función con la cual declaramos un test

```javascript
test("suma correcta", function(){
.... //codigo del test
});
```
con esto declaramos el test, el otro paso es verificar que el resultado es el esperado, para ello hay varias "aserciones"

```javascript
test("suma correcta", function(){
    var resultado = suma(3, 4);
    equal(resultado, 7, "El resultado no es el esperado");
});
```
de este modo verificamos que el resultado sea 7, sino el test fallará. Por supuesto existen más [aserciones](http://api.qunitjs.com/category/assert/).

[Ejemplo 2](https://github.com/leomicheloni/TDD-y-automatizaci-n-con-Javascript/blob/master/examples/example_2.js)

##TDD, desarrollo orientado por pruebas

El desarrollo orientado por pruebas (TDD) es comenzar nuestro código a partir de la prueba que verifica el comportamiento que vamos a implementar luego, por ejemplo:

```javascript
test("resta correcta", function(){
	var resultado = resta(8,5);
	equal(resultado, 3, "La resta es incorrecta");
});
```

En este caso escribimos el test para verificar la resta pero aún no hemos escrito nada de código, ni siquiera el método, el siguiete paso sería:

```javascript
function resta(a, b){

}
```

Y corremos el test una vez más, por supuesto fallará, la idea es ir avanzando de a poco o "baby steps", en este caso el siguiente paso podría ser:

```javascript
function resta(a, b){
	return a-b;
}
```

[Ejemplo 3](https://github.com/leomicheloni/TDD-y-automatizaci-n-con-Javascript/blob/master/examples/example_3.js)

Y listo. De este modo vamos avanzando sobre pasos seguros y el finalizar tenemos el código listo y probado.
En muchos casos vamos a escribir varias pruebas antes de comenzar a codificar lo que queremos probar.

![unit result](https://github.com/leomicheloni/TDD-y-automatizaci-n-con-Javascript/blob/master/screenshots/qunit.png)

###Ventajas de TDD
Algunas ventajas de TDD son:
 - Al terminar tenemos un conjunto de test para probar nuestro código y podemos correrlos durante todo el proceso de desarrollo.
 - Nos permite ir "descubriendo" nuestro sistema, es decir, a medida que pensamos las pruebas vamos descubriendo nuevos casos y repensando definiciones.

###Cómo automatizar QUnit?
Una de las dificultades para automatizar las pruebas hechas con QUnit es el hecho de que necesitamos un navegador web, pero esto no es necesariamente un problema ya que podemos utilizar [PhantomJs](http://phantomjs.org/) que es un navegador sin interfaz, y podemos invocarlos por línea de comandos.

##PhantomJs
Es lo que se conoce como "headless browser": un navegador sin interfaz gráfica, tiene muchas utilidades en nuestro caso vamos a correr las pruebas de QUnit y ver el resultado por consola.

Para utilizar PhantomJs tenemos que interactuar con su API, entonces si queremos correr los test de QUnit tenemos que escribir un script (se conocen como runners) para decirle que lo carge nuestra página de prueba y luego nos muestre el resultado, por suerte entre los ejemplos que se incluyen en la descarga de PhantomJs [existe uno](https://github.com/ariya/phantomjs/blob/master/examples/run-qunit.js)

Con un sencillo comando le decimos a PhantomJs que cargue el runner y le pasamos el nombre de nuestra página de pruebas como parámetro:

```bat
phantomjs.exe run-qunit.js examples\exampleloader.html
```

y vemos el resultado en la consola, genial

![phantom result](https://github.com/leomicheloni/TDD-y-automatizaci-n-con-Javascript/blob/master/screenshots/phatomresult.png)


##Automatizar las pruebas, testing continuo
En este caso con un lindo script hecho con PowerShell logramos que las pruebas de QUnit se ejecuten sobre PhantomJs cada vez que guardamos un archivo.

Para hacerlo podemos escribir un script que mire los cambios y ejecute el mismo comando de PhantomJs, en este caso voy a utilizar un script que hizo [mi buen amigo José](http://joseoncode.com/2011/08/08/javascript-continuous-testing-with-qunit-phantomjs-and-powershell/).

![phantom result](https://github.com/leomicheloni/TDD-y-automatizaci-n-con-Javascript/blob/master/screenshots/powershell.png)




