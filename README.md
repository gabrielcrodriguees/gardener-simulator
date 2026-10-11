# Garden Simulator

A turn-based garden simulator written in C++, where a gardener interacts with tools and several plant species in a grid-based garden.
The project was developed for the *Object-Oriented Programming (Programação Orientada a Objetos)* course, Computer Engineering degree, ISEC (group project of 2).

## Overview

The garden is a rectangular grid (up to 26x26) where each position has its own soil (water and nutrients) and can hold one plant and one tool. The user plays as the gardener, who can move around, plant, harvest and use tools through text commands.

Time is simulated in abstract "instants", advanced by the user. At each instant, plants consume resources, grow, reproduce or die depending on their species.

## Technologies Used

- **C++**: Core language
- **CLion / CMake**: Project and build tool
- **STL**: Only used for save/restore (`std::map`), never for the garden grid

## Architecture

The project follows an object-oriented design, with inheritance and polymorphism:

- **Jardim & Posicao**: The garden is a dynamic array of `Posicao` objects, simulating a 2D matrix through pointer arithmetic (no STL containers for the soil).
- **Planta** (abstract base class): Common interface for all plants (`passarInstante`, `estaVivo`, `clone`):
  - `Cacto`: Resistant, dies from excess water
  - `Roseira`: Demanding, dies if surrounded by other plants
  - `ErvaDaninha`: Parasitic, multiplies aggressively
  - `PlantaExotica`: Group-designed "altruistic" plant that donates water to neighboring positions
- **Ferramenta** (abstract base class): Common interface for all tools:
  - `Regador`: Adds water (consumable)
  - `PacoteAdubo`: Adds nutrients (consumable)
  - `TesouraPoda`: Removes "ugly" plants (indestructible)
  - `FerramentaZ`: Group-designed magnet that collects every tool in the garden
- **Jardineiro**: Player state (position, inventory, active tool) and per-turn action limits
- **Simulador, LeitorComandos, Visualizador**: Controller, command parsing and console display
- **Settings**: Static class holding every game parameter

### Key Design Choices

- **Polymorphic copy:** a virtual `clone()` method lets plants and tools be copied through a base class pointer, used for plant reproduction and for saves.
- **Save/Restore:** a `struct EstadoJardim` stores a copy of the garden and the gardener, kept in a `std::map<std::string, EstadoJardim*>` indexed by the name chosen by the user.

## Symbols

| Symbol | Meaning |
|--------|---------|
| `*` | Gardener |
| `c` | Cactus |
| `r` | Rose bush |
| `e` | Weed |
| `x` | Exotic plant |
| `g` | Watering can |
| `a` | Fertilizer pack |
| `t` | Pruning shears |
| `z` | Magnet tool |

The display priority is: gardener > plant > tool.

## Commands

| Command | Description |
|---------|-------------|
| `jardim <lines> <columns>` | Create the garden (must be the first command) |
| `avanca [n]` | Advance `n` instants |
| `lplantas` | List all plants |
| `lplanta <lc>` | List the plant at a position |
| `larea` | List all non-empty positions |
| `lsolo <lc> [n]` | List the soil at a position (or in a radius `n`) |
| `lferr` | List the gardener's tools |
| `colhe <lc>` | Harvest a plant |
| `planta <lc> <type>` | Plant a new plant (`c`, `r`, `e`, `x`) |
| `larga` / `pega <n>` | Drop / pick up a tool by serial number |
| `compra <type>` | Buy a tool (`g`, `a`, `t`, `z`) |
| `e` `d` `c` `b` | Move left, right, up, down |
| `entra <lc>` / `sai` | Enter / leave the garden |
| `grava <name>` | Save the current state |
| `recupera <name>` | Restore a saved state |
| `apaga <name>` | Delete a saved state |
| `executa <file>` | Run commands from a text file |
| `fim` | Exit the program |

Positions are written with letters: line then column (e.g. `aa` is the top-left corner).

## How to Run

1. Clone the repository:
```bash
   git clone https://github.com/gabrielcrodriguees/gardener-simulator.git
```
2. Open the project in CLion (or build it with CMake).
3. Run the program and start with the `jardim` command, e.g. `jardim 10 12`.

## Authors

- Gabriel Cardoso Rodrigues
- Artur Meireles de Sousa Falcão

ISEC, Instituto Politécnico de Coimbra
