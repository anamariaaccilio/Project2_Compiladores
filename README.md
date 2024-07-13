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
