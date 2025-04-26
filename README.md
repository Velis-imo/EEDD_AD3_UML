
# Sistema de gestion de Torneos

Se presenta la solución al problema de cómo gestionar el torneo nacional de Futbol Sala.

## 1. Análisis del problema y requisitos del sistema

¿Quiénes son los actores que interactúan con el sistema?

Los actores principales de la aplicacion de gestion de torneos son principalemente 3.

- Administrador del torneo.
- Jugador.
- Gestor de Rwesultados.

### 1.1 ¿Cuáles son las acciones que cada actor puede realizar?

- El actor "administrador del torneo" (exclusivamente), será el encargado gestionar el torneo a través de crear el torneo, registrar e inscribir equipos y jugadores (así como eliminarlos), hacer emparejamientos de partidas y entregar premios.

- El actor "jugador" podrá consultar la información del torneo tales como estadísticas, clasificaciones, rankings, etc.

- El actor "registrador de resultados" sera la parte ecnargada de registrar, almacenar y actualizar los datos, resultados, clasificaciones, rankings, estadisticas y demás información relevante del torneo

### 1.2 ¿Cómo se relacionan entre sí las entidades del sistema?

- Torneo

  - Compone a varios Equipo (un torneo tiene varios equipos; si el torneo se elimina, los equipos dejan de pertenecer a él).
  - Compone a varias Partida (un torneo tiene varias partidas; si el torneo se elimina, las partidas desaparecen).
  - Compone a  un Premio (los premios están ligados a un torneo).
  - Asocia a Equipo mediante la inscripción de equipos en el torneo.
  - Asocia a Premio con Equipo (los premios se asignan a equipos según los resultados).

- Equipo
  - Agrega a varios Jugador (un equipo puede tener varios jugadores; los jugadores pueden existir independientemente del equipo). Participa en varias Partida (un equipo juega varias partidas).

- Jugador
  - Pertenece a un Equipo (un jugador puede estar en un equipo).
  - No recibe Premio ya que premio se entrega sólo a Equipo.

- Partida
  - Asocia a dos Equipo (cada partida enfrenta a dos equipos).
  - Pertenece a un Torneo.

- Premio
  - Pertenece a un Torneo.
  - Se asigna a un Equipo.

## 2. Identificación de los casos de uso y elaboración de del diagrama

### 2.1 Casos de Uso y Detalles

- **Login:**

  - **¿Quién lo usa?** Jugador, Administrador de Torneo y Registrador de Resultados.
  - **¿Qué hace?** Permite a los diferentes usuarios conectarse a la plataforma segun sus categorías.
  - **Incluye:** Validar Dato: Comprueba y verifica los permisos de usuario.

- **Consultar infoTorneo:**

  - **¿Quién lo usa?** Jugador.
  - **¿Qué hace?** Permite al jugador ver datos del torneo: fechas, equipos, reglas, clasificaciones, resultados.
  

- **Asignar premios**

  - **¿Quién lo usa?** Administrador.
  - **¿Qué hace?** Permite al administrador asignar premios a los equipos ganadores según los resultados.
  

- **Inscribir equipos en torneo**
  - **¿Quién lo usa?** Administrador.
  - **¿Qué hace?** Permite agregar nuevos equipos al torneo.
  - **Incluye:** Comprobar Disponibilidad: Antes de inscribir, verifica que haya cupos disponibles y que el equipo cumpla los requisitos.

- **Gestionar Torneo**
  - **¿Quién lo usa?** Administrador.
  - **¿Qué hace?** Caso de uso general que agrupa todas las tareas administrativas del torneo.
  - **Incluye:** Comprobar Disponibilidad: Para cualquier gestión, primero verifica la disponibilidad de recursos o cupos.

- **Crear Torneo**
  - **¿Quién lo usa?** Administrador.
  - **¿Qué hace?** Permite crear un nuevo torneo en el sistema.
  - **Incluye:**. Comprobar Disponibilidad: Antes de crear, verifica que no haya conflictos de fechas, recursos, o superposiciones con otros torneos.

- **Inscribir jugador a equipo**
  - **¿Quién lo usa?** Administrador.
  - **¿Qué hace?** Permite agregar jugadores a un equipo.
  - **Incluye:**. Comprobar Disponibilidad: Verifica si el equipo tiene cupo y si el jugador puede ser inscrito.

- **Hacer emparejamientos**
  - **¿Quién lo usa?** Administrador.
  - **¿Qué hace?** Permite organizar los enfrentamientos entre los equipos participantes.
  - **Incluye:**. Comprobar Disponibilidad: Antes de emparejar, verifica que los equipos estén inscritos y disponibles.

- **Comprobar Disponibilidad**
  - **¿Quién lo usa?** Administrador (de manera indirecta, a través de otros casos de uso).
  - **¿Qué hace?** Administrador (de manera indirecta, a través de otros casos de uso).
  - **Incluye:** Se incluye en todos los procesos administrativos donde se debe validar disponibilidad antes de proceder.

- **Actualizar torneo: clasificación, resultados, etc.**
  - **¿Quién lo usa?** Administrador (de manera indirecta, a través de otros casos de uso).
  - **¿Qué hace?** Permite actualizar la información del torneo, como clasificaciones, resultados, y estado general.
  - **Incluye:** Registrar y Almacenar resultados de partidas: Para actualizar, primero debe registrar los resultados de las partidas jugadas.

- **Registrar y Almacenar resultados de partidas**
  - **¿Quién lo usa?** Gestor BBDD.
  - **¿Qué hace?** Permite actualizar la información del torneo, como clasificaciones, resultados, y estado general.
  - **Incluye:** uarda en la base de datos los resultados de las partidas jugadas, para que luego puedan ser consultados y usados en la actualización del torneo.

### 2.2 Creacion de diagrama de casos de uso

## 3. Identificación de clases y relaciones

### 3.1. Identificación las clases principales en función de los casos de uso seleccionados.

Las clases principales en función al uso son:

- Torneo
- Equipo
- Jugador
- Partida
- Premio
  
### 3.2 Distinción de clases de Entidad, Control e Interfaz para mantener una arquitectura modular. Se definen los atributos y métodos de las clases

Para este apartado del enunciado nos centraremos en el Patrón MVC (MODELO-VISTA-CONTROLADOR) ya que separa la lógica de la aplicación en tres componentes independientes para mejorar la organización, el mantenimiento y la escalabilidad del código.

#### 3.2.1 Entidad (Modelo)

##### **Torneo**

``` java
Atributos:
    - idTorneo: int
    - nombre: String
    - fechaInicio: Date
    - fechaFin: Date
    - estado: String
    - reglas: String
Métodos:
  + getters(),setters(), equals()...
  + comprobarDisponibilidad(): boolean
  + crearTorneo()
  + inscribirEquipo(equipo: Equipo)
  + asignarPremio(premio: Premio, equipo: Equipo)
  + emparejarEquipos(): List<Partida>
```

##### Equipo

```java
Atributos:
  - idEquipo: int
  - nombre: String
  - entrenador: String
Métodos:
  + getters(),setters(), equals()...
  + añadirJugadir(jugador: Jugador)
  + eliminarJugador(jugador: Jugador)
```

##### Jugador

```java
Atributos:
 - idJugador: int
 - nombre: String
 - nickname: String
 - edad: int
Métodos:
 + getters(),setters(), equals()...
 + verJugador()
```

##### Partida

```java
Atributos:
 - idPartida: int
 - fecha: Date
 - equipo1: idEquipo
 - equipo2: idEquipo
Métodos:
 + getters(),setters(), equals()...
 + obtenerResultado()
 + setData()
```

##### Premio

```java
Atributos:
 - idPremio: int
 - descripcion: String
 - tipo: String
 - valor: double
Métodos:
 + getters(),setters(), equals()...
 + verPremio()
```

#### 3.2.2 Control (Controlador)

##### ControlTorneo

```java
Atributos:
-List<Torneo>
-List<Equipo>
-List<Jugador>
-List<Partida>

Métodos: CRUD
+ CREATE,READ,UPDATE,DELETE: Torneo
+ CREATE,READ,UPDATE,DELETE: Equipo
+ CREATE,READ,UPDATE,DELETE: Jugador
+ CREATE,READ,UPDATE,DELETE: Partida
```

#### 3.2.3 Clases de Vista

##### UIAdministrador

```java
Métodos:
  +mostrarMenuAdmin()
  +solicitarDatosTorneo()
  +mostrarClasificacion()
  +mostrarPremios()
```

##### UIJugador

```java
Métodos:
  +mostrarInfoTorneo()
  +mostrarEquipos()
  +mostrarResultados()
```

##### UIGestorBBDD

```java
Métodos:
 +registrarResultado()
 +almacenarDatos()
```

### 3.3 Establece relaciones entre clases, asegurando la correcta representación de asociaciones, agregaciones y composiciones

- Torneo compone a Partida y a Premio (composición), es decir: si se eliminase un Torneo, las partidas y premios de ese torneo también desaparecerían ya que ambos (Premio y Partida), están ligadas a la vida del Torneo.
- Equipo agrega Jugador (agregación), es decir, si se eliminase un torneo, los equipos de ese torneo no se eliminan.
- Partida asocia a dos Equipo (asociación), es decir, que ambas se relacionan y relacionan por igual. 
- Premio asocia a Equipo (asociación).

## 4. Creación de diagramas de clases

Se han adjuntado dos diagramas dentro de la carpeta img/ del repositorio.

- Diagrmas de Casos de Uso.
- Diagrama de Clases.
