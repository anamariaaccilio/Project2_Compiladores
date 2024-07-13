# Project2_Compiladores

## Item 1

### Typechecker por función

En este apartado describiremos los cambios realizados para calcular el espacio requerido para almacenar las variables locales y la altura máxima de la pila, que serán necesarios para las instrucciones de pila "alloc" y "enter" respectivamente.

Primero, en imp_typechecker.hh creamos los siguientes atributos:
- int nivel: utilizado para determinar si estamos o no analizando una declaración de variable global.
- int var_globales: necesario para contar el número de variables globales.




![image](https://github.com/user-attachments/assets/3c60bd19-5a3d-47ff-9cc8-fe3aef25ac26)
