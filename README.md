[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/-g_ni1Wx)
# Documentación del Proyecto
## Unidad 3

Estudiante:  Jose Ignacio Trujillo Cano 
Id:  000335384
---


##  Abstracción 
Este fundamento es el encargado del proceso de simplificar sistemas complejos al representar solo lo esencial, ocultando detalles innecesarios. En C, esto se logra usando estructuras que modelan objetos y funciones que operan sobre ellos, organizadas en archivos .h para las declaraciones y .c para las implementaciones. Se realizaron las actividades propuestas con respecto al tema creando una clase llamada Arma con sus atributos e imprimiendo su información. 

##  Encapsulamiento
Este fundamento es el encargado de ocultar los detalles internos de una clase, exponiendo solo lo necesario a través de una interfaz pública, protegiendo los datos y facilitando su mantenimiento. En C, se implementa separando las declaraciones en archivos .h y las definiciones en .c, usando el modificador static para limitar la visibilidad de funciones y variables a un solo archivo. Se refactorizo el código de Arma que se creó en la anterior actividad para que las funciones y variables sean static y no se encuentren en archivo main 

##  Herencia
Este fundamento es el encargado de crear nuevas clases a partir de otras, reutilizando y extendiendo su funcionalidad. En C, se simula incluyendo una estructura dentro de otra, ya que no tiene soporte directo para herencia. Se realizo la actividad practica creando la estructura de Mago que heredara los atributos del personaje, también se sobrescribió la función de mostrar_estado para incluir un nuevo atributo que fue mana.	

## Polimorfismo
Este fundamento es el que permite que distintas clases se traten como una clase base, ejecutando funciones de forma diferente según la clase real del objeto. En C, se implementa usando punteros a la clase base y punteros a funciones. Ya se implementó en el código entendiendo sus características específicas.
Relaciones entre Clases
Por el momento no lo e añadido al código, pero la teoría es comprensible.

##  Conclusiones
La mayoría de las actividades practicas fueron elaboradas a satisfacción sin ningún inconveniente, además fueron añadidas al respectivo repositorio exceptuando la relación entre clase.
Hasta ahora, todo ha sido claro y no he tenido inconvenientes con la implementación de las actividades. Aunque aún no he terminado el último punto de relación entre clases, me he sentido cómodo trabajando y sin dificultades. Toda la documentación se encuentra en el repositorio.

##  Ejercicio realizado en clase

## Main 
 ```c
#include "personaje.h"
#include "guerrero.h"
#include "mago.h"
#include <stdio.h>


	int main(void)
	{
		Personaje* personajes[2];
		Guerrero* guerrero = Guerrero_crear("Arthas", 100, 10, 80);
		Mago* mago = mago_crear("Jaina", 80, 12, 120);

		personajes[0] = (Personaje*)guerrero;
		personajes[1] = (Personaje*)mago;

		for (int i = 0; i < 2; ++i) {
			personajes[i]->mostrar_estado(personajes[i]);
		}

		Guerrero_destruir(guerrero);
		mago_destruir(mago);

		Personaje *Mago, *Guerrero;
		Mago = Personaje_crear("Gandalf", 3, 10);//Reservar memoria
		printf("El mago se llama %s\n", get_name(Mago));
		Personaje_destruir(Mago); //Liberar memoria dinámica

		return 0;
	}
 ```

##  Personaje.h
```c
#ifndef PERSONAJE_H
#define PERSONAJE_H

typedef struct Personaje {
    char nombre[30];
    int vida;
    int nivel;
    void (*mostrar_estado)(const struct Personaje* this);
} Personaje;

Personaje* Personaje_crear(static const char* nombre, int vida, int nivel);
void Personaje_destruir(Personaje* this);
char* get_name(Personaje* this);


#endif // PERSONAJE_H
```
## Personaje.C
```c
#include "Personaje.h"
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

 void mostrar_estado_impl(const Personaje* this) {
    printf("Personaje: %s | Vida: %d | Nivel: %d\\n", this->nombre, this->vida, this->nivel);
}

Personaje* Personaje_crear(const char* nombre, int vida, int nivel) {
    Personaje* nuevo_personaje = (Personaje*)malloc(sizeof(Personaje));
    if (!nuevo_personaje) return NULL;
    //nuevo_personaje->nombre = strdup(nombre);
    strcpy_s(nuevo_personaje->nombre, 30, nombre);
    nuevo_personaje->vida = vida;
    nuevo_personaje->nivel = nivel;
    nuevo_personaje->mostrar_estado = mostrar_estado_impl;
    return nuevo_personaje;
}

char* get_name(Personaje* this) {
    return this->nombre;
}

void Personaje_destruir(Personaje* this) {
    if (this) {
        //free(this->nombre);
        free(this);
    }
}
```
## Guerrero.h
```c
#ifndef GUERRERO_H
#define GUERRERO_H

#include "Personaje.h"

typedef struct Guerrero {
    Personaje base;
    int fuerza;
} Guerrero;

Guerrero* Guerrero_crear(const char* nombre, int vida, int nivel, int fuerza);
void Guerrero_destruir(Guerrero* this);

#endif // GUERRERO_H
#pragma once
```
## Guerrero.C
```c
#include "Guerrero.h"
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

static void mostrar_estado_impl(const Personaje* this) {
    Guerrero* guerrero = (Guerrero*)this;
    printf("Guerrero: %s | Vida: %d | Nivel: %d | Fuerza: %d\n", this->nombre, this->vida, this->nivel, guerrero->fuerza);
}

Guerrero* Guerrero_crear(const char* nombre, int vida, int nivel, int fuerza) {
    Guerrero* nuevo_guerrero = (Guerrero*)malloc(sizeof(Guerrero));
    if (!nuevo_guerrero) return NULL;
    strcpy_s(nuevo_guerrero->base.nombre, 30, nombre);
    nuevo_guerrero->base.vida = vida;
    nuevo_guerrero->base.nivel = nivel;
    nuevo_guerrero->fuerza = fuerza;
    nuevo_guerrero->base.mostrar_estado = mostrar_estado_impl;
    return nuevo_guerrero;
}

void Guerrero_destruir(Guerrero* this) {
    if (this) {
        free(this);
    }
}
```
## Mago.c
```c
#include "mago.h"
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

static void mostrar_estado_impl(const Personaje* this) {
    Mago* mago = (Mago*)this;
    printf("Mago: %s | Vida: %d | Nivel: %d | Mana: %d\n", this->nombre, this->vida, this->nivel, mago->Mana);
}

Mago* mago_crear(const char* nombre, int vida, int nivel, int mana) {
    Mago* nuevo_mago = (Mago*)malloc(sizeof(Mago));
    if (!nuevo_mago) return NULL;
    strcpy_s(nuevo_mago->base.nombre, 30, nombre);
    nuevo_mago->base.vida = vida;
    nuevo_mago->base.nivel = nivel;
    nuevo_mago->Mana = mana;
    nuevo_mago->base.mostrar_estado = mostrar_estado_impl;
    return nuevo_mago;
}

void mago_destruir(Mago* this) {
    if (this) {
        //free(this->base.nombre);
        free(this);
    }
}
```
## Mago.h
```c
#ifndef MAGO_H
#define MAGO_H

#include "personaje.h"


typedef struct Mago {
    Personaje base;
    int Mana;
} Mago;

Mago* mago_crear(const char* nombre, int vida, int nivel, int fuerza);
void mago_destruir(Mago* this);

#endif // GUERRERO_H
```
