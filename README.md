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
## ACTIVIDAD 2 

## Codigo anterior del juego del laberinto 
```c
#include <SDL.h>
#include <stdio.h>
#include <stdbool.h>

#define WINDOW_WIDTH 640
#define WINDOW_HEIGHT 480
#define LINE_SIZE 40  
#define PLAYER_SIZE 30 
#define PLAYER_OFFSET (LINE_SIZE - PLAYER_SIZE) / 2
#define PLAYER_SPEED 5  
#define FPS 60
#define FRAME_TARGET_TIME (1000 / FPS)

SDL_Window* window = NULL;
SDL_Renderer* renderer = NULL;
bool gameIsRunning = true;
int lastUpdateTime = 0;

int maze[12][16] = {
    {1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1},
    {1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 1},
    {1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 1, 0, 1},
    {1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 1},
    {1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1},
    {1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1},
    {1, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1},
    {1, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1},
    {1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1},
    {1, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1},
    {1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1}
};

int playerX = 1 * LINE_SIZE + PLAYER_OFFSET;
int playerY = 1 * LINE_SIZE + PLAYER_OFFSET;


void Setup() {
    SDL_Init(SDL_INIT_VIDEO);
    window = SDL_CreateWindow("Juego de Laberinto", SDL_WINDOWPOS_CENTERED, SDL_WINDOWPOS_CENTERED, WINDOW_WIDTH, WINDOW_HEIGHT, 0);
    renderer = SDL_CreateRenderer(window, -1, SDL_RENDERER_ACCELERATED);
}

void Update() {
    int time_to_wait = FRAME_TARGET_TIME - (SDL_GetTicks() - lastUpdateTime);
    if (time_to_wait > 0 && time_to_wait <= FRAME_TARGET_TIME) {
        SDL_Delay(time_to_wait);
    }
    lastUpdateTime = SDL_GetTicks();
}

void Render() {
    SDL_SetRenderDrawColor(renderer, 0, 0, 0, 255);
    SDL_RenderClear(renderer);

    for (int row = 0; row < 12; row++) {
        for (int col = 0; col < 16; col++) {
            if (maze[row][col] == 1) {
                SDL_SetRenderDrawColor(renderer, 255, 255, 255, 255);
                SDL_Rect wall = { col * LINE_SIZE, row * LINE_SIZE, LINE_SIZE, LINE_SIZE };
                SDL_RenderFillRect(renderer, &wall);
            }
        }
    }

    SDL_SetRenderDrawColor(renderer, 255, 0, 0, 255);
    SDL_Rect player = { playerX, playerY, PLAYER_SIZE, PLAYER_SIZE };
    SDL_RenderFillRect(renderer, &player);

    SDL_RenderPresent(renderer);
}

bool CheckCollision(int newX, int newY) {
    SDL_Rect playerRect = { newX, newY, PLAYER_SIZE, PLAYER_SIZE };

    for (int row = 0; row < 12; row++) {
        for (int col = 0; col < 16; col++) {
            if (maze[row][col] == 1) {
                SDL_Rect wallRect = { col * LINE_SIZE, row * LINE_SIZE, LINE_SIZE, LINE_SIZE };
                if (SDL_HasIntersection(&playerRect, &wallRect)) {
                    return true;
                }
            }
        }
    }

    return false;
}

void HandleEvents() {
    SDL_Event event;
    while (SDL_PollEvent(&event)) {
        if (event.type == SDL_QUIT) {
            gameIsRunning = false;
        }
    }

    const Uint8* state = SDL_GetKeyboardState(NULL);

    int newPlayerX = playerX;
    int newPlayerY = playerY;

    if (state[SDL_SCANCODE_UP]) {
        newPlayerY -= PLAYER_SPEED;
    }
    if (state[SDL_SCANCODE_DOWN]) {
        newPlayerY += PLAYER_SPEED;
    }
    if (state[SDL_SCANCODE_LEFT]) {
        newPlayerX -= PLAYER_SPEED;
    }
    if (state[SDL_SCANCODE_RIGHT]) {
        newPlayerX += PLAYER_SPEED;
    }

    if (!CheckCollision(newPlayerX, newPlayerY)) {
        playerX = newPlayerX;
        playerY = newPlayerY;
    }
}

int main(int argc, char* argv[]) {
    Setup();

    while (gameIsRunning) {
        HandleEvents();
        Update();
        Render();
    }

    SDL_DestroyRenderer(renderer);
    SDL_DestroyWindow(window);
    SDL_Quit();

    return 0;
}
```
## DIAGRAMA DE CLASES DEL JUEGO LABERINTO

![image](https://github.com/user-attachments/assets/515c3b55-7aab-434a-9dbb-1eb0a1f819fc)


## EXPLICACIÓN 
- Game interactúa directamente con Player y Laberinto. Se encarga de actualizar el estado del jugador y del laberinto en cada ciclo y coordina el renderizado de ambos.
- Player y Laberinto tienen métodos para verificar colisiones, pero es Game quien orquesta estas interacciones y asegura que el jugador no traspase las paredes del laberinto.

Herencia no se aplicó porque no hay una jerarquía natural ni una necesidad de compartir código entre clases mediante herencia. Cada clase tiene un propósito específico y aislado, por lo que es mejor usar composición y dependencia para estructurar las interacciones.

Agregación se utiliza entre Game y Player porque Player es un objeto que Game usa pero que puede existir fuera del juego. Esto permite una relación más flexible entre ambos.

Composición se usa entre Game y Maze porque el laberinto es una parte integral del juego, y su existencia depende de la instancia de Game. Si destruyes el juego, destruyes el laberinto.

Dependencia se usa entre Player y Maze ya que Player necesita al Maze para verificar el entorno (colisiones), pero no lo contiene ni lo controla.

## Main.c
```c
#include "game.h"
#include <SDL.h>
#include <stdio.h>
#include <stdbool.h>

#define WINDOW_WIDTH 640
#define WINDOW_HEIGHT 480
#define LINE_SIZE 40  
#define PLAYER_SIZE 30 
#define PLAYER_OFFSET (LINE_SIZE - PLAYER_SIZE) / 2
#define PLAYER_SPEED 5  
#define FPS 60
#define FRAME_TARGET_TIME (1000 / FPS)

int main(int argc, char* argv[]) {
    Game game;
    game.Run();
    return 0;
}
```
## Player.c
```c
#include "player.h"
#include <SDL.h>

// Constructor
Player::Player(float x, float y) : Entity(x, y), health(100), score(0) {
    // Inicializamos al jugador con su posición y atributos iniciales
}

// Método para actualizar el estado del jugador (se sobrescribe de Entity)
void Player::Update() {
    // Aquí podrías agregar lógica adicional para actualizar el jugador (colisiones, etc.)
}

// Método para renderizar al jugador en la pantalla (se sobrescribe de Entity)
void Player::Render(SDL_Renderer* renderer) {
    SDL_SetRenderDrawColor(renderer, 255, 0, 0, 255);  // Color rojo para el jugador
    SDL_Rect playerRect = { static_cast<int>(position.x), static_cast<int>(position.y), PLAYER_SIZE, PLAYER_SIZE };
    SDL_RenderFillRect(renderer, &playerRect);  // Dibujar el rectángulo del jugador
}

// Maneja la entrada del jugador (teclas presionadas)
void Player::HandleInput(const Uint8* state) {
    Vector2D newPosition = position;

    if (state[SDL_SCANCODE_UP]) newPosition.y -= PLAYER_SPEED;
    if (state[SDL_SCANCODE_DOWN]) newPosition.y += PLAYER_SPEED;
    if (state[SDL_SCANCODE_LEFT]) newPosition.x -= PLAYER_SPEED;
    if (state[SDL_SCANCODE_RIGHT]) newPosition.x += PLAYER_SPEED;

    SetPosition(newPosition.x, newPosition.y);
}

// Getters y setters para health
int Player::GetHealth() const {
    return health;
}

void Player::SetHealth(int health) {
    this->health = health;
}

// Getters y setters para score
int Player::GetScore() const {
    return score;
}

void Player::SetScore(int score) {
    this->score = score;
}

#include "player.h"
#include <SDL.h>

// Constructor
Player::Player(float x, float y) : Entity(x, y), health(100), score(0) {
    // Inicializamos al jugador con su posición y atributos iniciales
}

// Método para actualizar el estado del jugador (se sobrescribe de Entity)
void Player::Update() {
    // Aquí podrías agregar lógica adicional para actualizar el jugador (colisiones, etc.)
}

// Método para renderizar al jugador en la pantalla (se sobrescribe de Entity)
void Player::Render(SDL_Renderer* renderer) {
    SDL_SetRenderDrawColor(renderer, 255, 0, 0, 255);  // Color rojo para el jugador
    SDL_Rect playerRect = { static_cast<int>(position.x), static_cast<int>(position.y), PLAYER_SIZE, PLAYER_SIZE };
    SDL_RenderFillRect(renderer, &playerRect);  // Dibujar el rectángulo del jugador
}

// Maneja la entrada del jugador (teclas presionadas)
void Player::HandleInput(const Uint8* state) {
    Vector2D newPosition = position;

    if (state[SDL_SCANCODE_UP]) newPosition.y -= PLAYER_SPEED;
    if (state[SDL_SCANCODE_DOWN]) newPosition.y += PLAYER_SPEED;
    if (state[SDL_SCANCODE_LEFT]) newPosition.x -= PLAYER_SPEED;
    if (state[SDL_SCANCODE_RIGHT]) newPosition.x += PLAYER_SPEED;

    SetPosition(newPosition.x, newPosition.y);
}

// Getters y setters para health
int Player::GetHealth() const {
    return health;
}

void Player::SetHealth(int health) {
    this->health = health;
}

// Getters y setters para score
int Player::GetScore() const {
    return score;
}

void Player::SetScore(int score) {
    this->score = score;
}
```
## Entity.c
```c
#include "entity.h"
#include <SDL.h>

// Constructor de Entity
Entity::Entity(float x, float y) : position(x, y), texture(nullptr) {}

// Método para obtener la posición
Vector2D Entity::GetPosition() const {
    return position;
}

// Método para establecer la posición
void Entity::SetPosition(float x, float y) {
    position.Set(x, y);
}
```
## Game.c
```c
#include "game.h"
#include <SDL.h>

Game::Game() : window(nullptr), renderer(nullptr), gameIsRunning(true), lastUpdateTime(0) {
    player = new Player(1 * LINE_SIZE + PLAYER_OFFSET, 1 * LINE_SIZE + PLAYER_OFFSET);
}

Game::~Game() {
    delete player;
    SDL_DestroyRenderer(renderer);
    SDL_DestroyWindow(window);
    SDL_Quit();
}

void Game::Setup() {
    SDL_Init(SDL_INIT_VIDEO);
    window = SDL_CreateWindow("Juego de Laberinto", SDL_WINDOWPOS_CENTERED, SDL_WINDOWPOS_CENTERED, WINDOW_WIDTH, WINDOW_HEIGHT, 0);
    renderer = SDL_CreateRenderer(window, -1, SDL_RENDERER_ACCELERATED);
}

void Game::Update() {
    int time_to_wait = FRAME_TARGET_TIME - (SDL_GetTicks() - lastUpdateTime);
    if (time_to_wait > 0 && time_to_wait <= FRAME_TARGET_TIME) {
        SDL_Delay(time_to_wait);
    }
    lastUpdateTime = SDL_GetTicks();
}

void Game::Render() {
    SDL_SetRenderDrawColor(renderer, 0, 0, 0, 255);
    SDL_RenderClear(renderer);

    maze.Draw(renderer);
    player->Render(renderer);

    SDL_RenderPresent(renderer);
}

void Game::HandleEvents() {
    SDL_Event event;
    while (SDL_PollEvent(&event)) {
        if (event.type == SDL_QUIT) {
            gameIsRunning = false;
        }
    }

    const Uint8* state = SDL_GetKeyboardState(NULL);
    player->HandleInput(state);

    if (maze.CheckCollision(player)) {
        // Manejar la colisión
    }
}

void Game::Run() {
    Setup();
    while (gameIsRunning) {
        HandleEvents();
        Update();
        Render();
    }
}
```
## Maze.c
```c
#include "maze.h"
#include <SDL.h>

class Maze {
private:
    int maze[12][16] = {
        {1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1},
        {1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 1},
        {1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 1, 0, 1},
        {1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 1},
        {1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1},
        {1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1},
        {1, 1, 1, 1, 1, 1, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1},
        {1, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1},
        {1, 0, 1, 1, 1, 1, 1, 1, 0, 1, 0, 1, 0, 1, 0, 1},
        {1, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1},
        {1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1}
    };

public:
    void Draw(SDL_Renderer* renderer) {
        SDL_SetRenderDrawColor(renderer, 255, 255, 255, 255);
        for (int row = 0; row < 12; row++) {
            for (int col = 0; col < 16; col++) {
                if (maze[row][col] == 1) {
                    SDL_Rect wallRect = { col * LINE_SIZE, row * LINE_SIZE, LINE_SIZE, LINE_SIZE };
                    SDL_RenderFillRect(renderer, &wallRect);
                }
            }
        }
    }

    bool CheckCollision(Entity* entity) {
        SDL_Rect entityRect = { static_cast<int>(entity->GetPosition().x), static_cast<int>(entity->GetPosition().y), PLAYER_SIZE, PLAYER_SIZE };

        for (int row = 0; row < 12; row++) {
            for (int col = 0; col < 16; col++) {
                if (maze[row][col] == 1) {
                    SDL_Rect wallRect = { col * LINE_SIZE, row * LINE_SIZE, LINE_SIZE, LINE_SIZE };
                    if (SDL_HasIntersection(&entityRect, &wallRect)) {
                        return true;
                    }
                }
            }
        }

        return false;
    }
};
```
## Vector2D.c
```c
#include "vector2d.h"
#include <cmath>  // Para sqrt en el cálculo de magnitud

// Constructor con parámetros
Vector2D::Vector2D(float x, float y) : x(x), y(y) {}

// Método para sumar otro vector a este
void Vector2D::Add(const Vector2D& other) {
    x += other.x;
    y += other.y;
}

// Método para establecer nuevos valores en el vector
void Vector2D::Set(float newX, float newY) {
    x = newX;
    y = newY;
}

// Método para calcular la magnitud del vector
float Vector2D::Magnitude() const {
    return sqrt(x * x + y * y);  // Raíz cuadrada de la suma de los cuadrados de x e y
}

// Método para normalizar el vector (convertirlo en un vector unitario)
Vector2D Vector2D::Normalize() const {
    float magnitude = Magnitude();
    if (magnitude == 0) {
        return Vector2D(0, 0);  // Evitar división por cero
    }
    return Vector2D(x / magnitude, y / magnitude);
}
```
## Entity.h
```c
#ifndef ENTITY_H
#define ENTITY_H

#include "Vector2D.h"  // Usamos Vector2D para la posición

class Entity {
protected:
    Vector2D position;  // Posición de la entidad
    SDL_Texture* texture;

public:
    // Constructor
    Entity(float x = 0, float y = 0);

    // Métodos virtuales para que puedan ser sobreescritos en Player
    virtual void Update() = 0;
    virtual void Render(SDL_Renderer* renderer) = 0;

    // Métodos de acceso
    Vector2D GetPosition() const;
    void SetPosition(float x, float y);
};

#endif // ENTITY_H

```
## Player.h
```c
#ifndef PLAYER_H
#define PLAYER_H

#include <SDL.h>
#include "Entity.h"  // Incluimos la clase base Entity
#include "Vector2D.h"  // Incluimos la clase Vector2D para manejar la posición

#define PLAYER_SIZE 30
#define PLAYER_SPEED 5

class Player : public Entity {
private:
    int health;
    int score;

public:
    // Constructor
    Player(float x, float y);

    // Métodos heredados de Entity que deben ser implementados
    void Update() override;
    void Render(SDL_Renderer* renderer) override;

    // Métodos adicionales específicos del jugador
    void HandleInput(const Uint8* state);

    // Getters y setters para atributos adicionales
    int GetHealth() const;
    void SetHealth(int health);

    int GetScore() const;
    void SetScore(int score);
};

#endif // PLAYER_H
```
## Game.h
```c
#ifndef GAME_H
#define GAME_H

#include <SDL.h>
#include "Player.h"  // Incluye la clase Player
#include "Maze.h"    // Incluye la clase Maze

class Game {
private:
    SDL_Window* window;
    SDL_Renderer* renderer;
    bool gameIsRunning;
    int lastUpdateTime;
    Player* player;  // Agregación: Game "tiene un" Player
    Maze maze;       // Agregación: Game "tiene un" Maze

public:
    Game();
    ~Game();
    void Setup();
    void Update();
    void Render();
    void HandleEvents();
    void Run();
};

#endif // GAME_H
```
## Vector2d.h
```c
#ifndef VECTOR2D_H
#define VECTOR2D_H

class Vector2D {
public:
    float x, y;

    // Constructor por defecto y constructor con parámetros
    Vector2D(float x = 0, float y = 0);

    // Métodos para operar con el vector
    void Add(const Vector2D& other);
    void Set(float newX, float newY);

    // Métodos adicionales útiles
    float Magnitude() const;         // Devuelve la magnitud del vector
    Vector2D Normalize() const;      // Normaliza el vector (lo convierte en un vector unitario)
};

#endif // VECTOR2D_H
```

![image](https://github.com/user-attachments/assets/0eb6cfbe-54e0-4db2-bba2-6bdaba4056dc)

