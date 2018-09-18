Erlang
======
% Signo de porcentaje inicia un comentario de una línea.


**Hola = hola.
OtherNode = ejemplo @ nodo.**

%% Se usarán dos por ciento de caracteres para comentar funciones.

%%% Se usarán tres por ciento de caracteres para comentar los módulos.

% Utilizamos tres tipos de puntuación en Erlang.
% Comas (`,`) argumentos separados en llamadas a funciones, constructores de datos y
% patrones.
% Periodos (`.`) (seguido de espacios en blanco) separa funciones completas y
% expresiones en el shell.
% Semicolons (`;`) cláusulas separadas. Encontramos cláusulas en varios contextos:
% de definiciones de funciones y en `case`,` if`, `try..catch`, y` receive`
% de expresiones

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%% %%
%% 1. Variables y coincidencia de patrones.
===========================================
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%% %%
% En Erlang, las nuevas variables están vinculadas con una instrucción `=`.
Num = 42.% Todos los nombres de variables deben comenzar con una letra mayúscula.

% Erlang tiene variables de asignación única; si intentas asignar un diferente
% de valor a la variable `Num`, obtendrá un error.
Num = 43.% ** error de excepción: no coincide con el valor del lado derecho 43

% En la mayoría de los idiomas, `=` denota una declaración de asignación. En Erlang, sin embargo,
% `=` denota una operación de coincidencia de patrones. Cuando se usa una variable vacía en el
% del lado izquierdo del operador `=` to está vinculado (asignado), pero cuando está atado
% variable se usa en el lado izquierdo, se observa el siguiente comportamiento.
% `Lhs = Rhs` realmente significa esto: evaluar el lado derecho (` Rhs`), y luego
% coincide con el resultado contra el patrón en el lado izquierdo (`Lhs`).
Num = 7 * 6.

% Número de punto flotante.
Pi = 3.14159.

% Los átomos se usan para representar diferentes valores constantes no numéricos. Átomos
% comienza con letras minúsculas, seguido de una secuencia de caracteres alfanuméricos
% de caracteres o el signo de subrayado (`_`) o en (` @ `).

Hola = hola.
OtherNode = ejemplo @ nodo.

% Los átomos con valores no alfanuméricos se pueden escribir al encerrar los átomos
% con apóstrofes.
AtomWithSpace = 'algún átomo con espacio'.

% Tuples son similares a las estructuras en C.
Point = {point, 10, 45}.

% Si queremos extraer algunos valores de una tupla, usamos el patrón de coincidencia
% operador `=`.
{punto, X, Y} = Punto. % X = 10, Y = 45

% Podemos usar `_` como marcador de posición para variables que no nos interesan.
% El símbolo `_` se llama una variable anónima. A diferencia de las variables regulares,
% varias apariciones de `_` en el mismo patrón no tienen que vincularse a
% mismo valor.
Person = {person, {name, {first, joe}, {last, armstrong}}, {footsize, 42}}.
{_, {_, {_, Quién}, _}, _} = Persona. % Who = joe

% Creamos una lista al encerrar los elementos de la lista entre corchetes y
% separándolos con comas.
% Los elementos individuales de una lista pueden ser de cualquier tipo.
% El primer elemento de una lista es el encabezado de la lista. Si te imaginas eliminar
% del encabezado de la lista, lo que queda se llama cola de la lista.
ThingsToBuy = [{manzanas, 10}, {peras, 6}, {leche, 3}].

% Si `T` es una lista, entonces` [H | T] `también es una lista, con la cabeza` H` y la cola `T`.
% La barra vertical (`|`) separa el encabezado de una lista de su cola.
% `[]` es la lista vacía.
% Podemos extraer elementos de una lista con una operación de coincidencia de patrones. Si nosotros
% tiene una lista no vacía `L`, luego la expresión` [X | Y] = L`, donde `X` y` Y`
% son variables independientes, extraerán el encabezado de la lista en `X` y la cola
% de la lista en `Y`.
[FirstThing | OtherThingsToBuy] = ThingsToBuy.
% FirstThing = {manzanas, 10}
% OtherThingsToBuy = [{peras, 6}, {leche, 3}]

% No hay cadenas en Erlang. Las cadenas son realmente solo listas de enteros.
% Las cadenas están entre comillas dobles (`" `).
Nombre = "Hola".
[72, 101, 108, 108, 111] = "Hola"
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%% %%
%% 2. Programación secuencial.
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%% %%

% Los módulos son la unidad básica de código en Erlang. Todas las funciones que escribimos son
% almacenado en módulos. Los módulos se almacenan en archivos con extensiones `.erl`.
% Los módulos deben compilarse antes de poder ejecutar el código. Un módulo compilado tiene el
% extensión `.beam`.
-módulo (geometría).
-export ([area / 1]). % de la lista de funciones exportadas desde el módulo.

% La función `área` consta de dos cláusulas. Las cláusulas están separadas por un
% punto y coma, y ​​la cláusula final termina con punto-espacio en blanco.
% Cada cláusula tiene una cabeza y un cuerpo; la cabeza consiste en un nombre de función
% seguido de un patrón (entre paréntesis), y el cuerpo consiste en una secuencia de
% expresiones, que se evalúan si el patrón en la cabeza es exitoso
% coincide con los argumentos de llamada. Los patrones se combinan en el orden
% aparecen en la definición de la función.
área ({rectángulo, ancho, Ht}) -> ancho * Ht;
área ({círculo, R}) -> 3.14159 * R * R.

% Compila el código en el archivo geometry.erl.
c (geometría). % {ok, geometría}

% Necesitamos incluir el nombre del módulo junto con el nombre de la función para
% identifica exactamente qué función queremos llamar.
geometría: área ({rectángulo, 10, 5}). % 50
geometría: área ({círculo, 1.4}). % 6.15752

% En Erlang, dos funciones con el mismo nombre y arity diferente (número de
% argumentos) en el mismo módulo representan funciones completamente diferentes.
-module (lib_misc).
-export ([sum / 1]). % función de exportación `suma` de arity 1
                  % acepta un argumento: lista de enteros.
suma (L) -> suma (L, 0).
suma ([], N) -> N;
suma ([H | T], N) -> suma (T, H + N).

% Funs son funciones "anónimas". Se llaman así porque tienen
% sin nombre. Sin embargo, pueden asignarse a variables.
Doble = diversión (X) -> 2 * X final. % `Doble` apunta a una función anónima
                              % con el controlador: #Fun <erl_eval.6.17052888>
Doble (2). % 4
% Functions acepta funs como sus argumentos y puede devolver funs.
Mult = diversión (Times) -> (fun (X) -> X * Times end) end.
Triple = Mult (3).
Triple (5). % 15

% Las listas de comprensión son expresiones que crean listas sin tener que usar
% funs, mapas o filtros.
% La notación `[F (X) || X <- L] `significa" la lista de `F (X)` donde se toma `X`
% de la lista `L`."
L = [1,2,3,4,5].
[2 * X || X <- L]. % [2,4,6,8,10]
% Una lista de comprensión puede tener generadores y filtros, que seleccionan un subconjunto de
% de los valores generados
EvenNumbers = [N || N <- [1, 2, 3, 4], N rem 2 == 0]. % [2, 4]

% Los protectores son construcciones que podemos usar para aumentar el poder del patrón
% coincidencia. Usando guardias, podemos realizar pruebas simples y comparaciones en el
% de variables en un patrón.
% Puede usar guardias en la cabeza de las definiciones de funciones donde están
% introducido por la palabra clave `when`, o puede usarlos en cualquier lugar del
% lenguaje donde se permite una expresión.
max (X, Y) cuando X> Y -> X;
max (X, Y) -> Y.

% Un guardia es una serie de expresiones de guardia, separadas por comas (`,`).
% La guardia `GuardExpr1, GuardExpr2, ..., GuardExprN` es verdadera si todos los guardias
% expresiones `GuardExpr1`,` GuardExpr2`, ..., `GuardExprN` evalúan` verdadero`.
is_cat (A) cuando is_atom (A), A =: = cat -> verdadero;
is_cat (A) -> falso.
is_dog (A) cuando is_atom (A), A =: = dog -> verdadero;
is_dog (A) -> falso.
