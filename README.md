# Project2_Compiladores

## Item 1

### Typechecker por función

En este apartado describiremos los cambios realizados para calcular el espacio requerido para almacenar las variables locales y la altura máxima de la pila, que serán necesarios para las instrucciones de pila "alloc" y "enter" respectivamente.

Primero, en imp_typechecker.hh creamos los siguientes atributos:
- int nivel: utilizado para determinar si estamos o no analizando una declaración de variable global.
- int var_globales: necesario para contar el número de variables globales.

Segundo, en el mismo archivo creamos los métodos:
- sp_decr: decrementa el SP
- sp_incr: incrementa el SP
- dir_decr: decrementa la dirección de memoria actual
- dir_incr: incrementa la dirección de memoria actual


![image](https://github.com/user-attachments/assets/3c60bd19-5a3d-47ff-9cc8-fe3aef25ac26)


Tercero, para realizar el conteo de variables globales se realiza en ImpTypeChecker::visit(VarDec* vd) verificando si el "nivel==1":

![image](https://github.com/user-attachments/assets/a536ab0e-6641-4200-bb5b-26ef7dc9027f)


Cuarto, para contar el número de variables locales por función modificamos ImpTypeChecker::visit(FunDecList* s) de la siguiente manera:

![image](https://github.com/user-attachments/assets/c30b7062-108b-43f8-960d-1b96e1f5d3c7)


Por último, al final del programa, el máximo tamaño de la pila para una función se calcula como #variables_locales + #variables_globales y se guarda en fentry.max_stack. Por su parte, el espacio requerido para las variables locales se guarda en fentry.mem_locals:

![image](https://github.com/user-attachments/assets/afc09d7b-8fee-4d16-be3e-c360f164f1b1)

### Direcciones de variables

#### Diferenciando variables locales de globales

Partimos del método ImpCodeGen::visit(Program* p), en donde se inicializa current_dir=1 y se clasifican las variables como globales o locales utilizando la variable process_global.

Toda declaración de variables (tanto locales como globales) y su respectiva de asignación de dirección de memoria se realiza en el método ImpCodeGen::visit(VarDec* vd) de la siguiente manera:

![image](https://github.com/user-attachments/assets/d6eec15b-3d9e-4e65-bdf7-0beaea61bb69)

Para reiniciar el valor de current_dir cada vez que acabamos de recorrer una función, se le asigna a current_dir=1 en ImpCodeGen::visit(FunDec* fd):

![image](https://github.com/user-attachments/assets/da26be50-2049-454c-b342-00958e5b7a6d)

De esta manera, todas las declaraciones de variables para cada función empiezan en la dirección de memoria 1. La forma en la que se distinguirá el acceso a memoria a direcciones de variables locales o globales será mediante el uso de store o storer según sea el caso:

![image](https://github.com/user-attachments/assets/b2fb1a36-23bc-4043-9901-a4ae5fa66922)


#### Dirección de parámetros de una función

Como vimos en clase de teoría, los parámetros se enumeran desde i=1 hasta n, siendo n el número de parámetros. La convención a la que se llegó es que la dirección en la que se debe guardar el parámetro i es i-(#parámetros+3). Este cálculo se realizó en ImpCodeGen::visit(FunDec* fd):

![image](https://github.com/user-attachments/assets/cc1e7f11-5f7f-4cd9-a429-c7f4e9b493a1)

#### Dirección del valor de retorno

Otra convención que se hizo en el curso es que el valor de retorno de una función se almacena siempre en -1*(#parámetros+3). Este cálculo se hizo también en ImpCodeGen::visit(FunDec* fd):

![image](https://github.com/user-attachments/assets/c9216a46-a708-4487-9911-072c2c2adb62)

