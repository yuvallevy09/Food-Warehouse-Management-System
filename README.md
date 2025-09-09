# Food Warehouse Management System (C++)

A step-based simulation that models how a community food warehouse fulfills orders using volunteers. Built as part of the Systems Programming course to demonstrate clean object‑oriented design, resource management, and modern C++ practices.

![Order handling flow](./order_handling_policy%202.png)

## Overview
- Customers place orders that are collected and delivered by volunteers.
- The simulation advances in steps and tracks each order from request to completion.
- Designed with clear separation of concerns, robust memory management (Rule of 5), and STL containers.

## Features
- **Step-based simulation**: progress the system deterministically and observe assignments and completions.
- **Config-driven setup**: initialize customers and volunteers from a file; add customers and orders via actions at runtime.
- **State persistence**: backup and restore the entire warehouse state.
- **Comprehensive status reporting**: print order, customer, volunteer, and action log statuses at any time.
- **Graceful close**: print final state and free resources.

## Highlights
- **OOP design**: `Warehouse`, `Order`, `Customer`, `Volunteer`, and `Action` hierarchies.
- **Volunteers**: Collectors and Drivers, with limited-capacity variants and realistic constraints (cooldown, distance, max orders).
- **Order lifecycle**: `PENDING → COLLECTING → DELIVERING → COMPLETED`.
- **Action-driven interface**: simulate steps, add customers/orders, print statuses, backup/restore state, and graceful close.
- **C++11 + STL**: adherence to RAII and Rule of 5; validated with `valgrind` for zero leaks.

## Architecture
- **State machine** controls order progression across `PENDING`, `COLLECTING`, `DELIVERING`, `COMPLETED`.
- **Command pattern** implemented via `BaseAction` and concrete actions (e.g., `SimulateStep`, `AddOrder`, `AddCustomer`, `Print*`, `Close`, `Backup`, `Restore`).
- **Separation of concerns**: `Warehouse` orchestrates; domain entities encapsulate behavior; actions mutate state.

## Key Entities
- **Warehouse**: central coordinator; holds customers, volunteers, and orders (pending/in-process/completed).
- **Customer**: `SoldierCustomer` or `CivilianCustomer`, each with constraints like distance and max orders.
- **Volunteer (abstract)**:
  - Collector / LimitedCollector (time per order, optional max orders)
  - Driver / LimitedDriver (max distance, distance per step, optional max orders)
- **Order**: maintains IDs, status, and assigned volunteers.
- **BaseAction (abstract)**: commands executed by the simulation (e.g., `SimulateStep`, `AddOrder`, `Print*`, `Close`, `Backup`, `Restore`).

## Tech & Skills Used
- **Programming**: C++11, STL containers/algorithms, RAII, Rule of 5
- **Design**: OOP, inheritance & polymorphism, Command pattern, state machine modeling
- **Tooling**: make, g++, valgrind (verified leak-free)
- **Practices**: separation of concerns, clean ownership semantics, config-file parsing, defensive error handling

## Build & Run
Requirements: `g++` (C++11) and `make`.

```bash
make            # builds to bin/warehouse and copies example config
./bin/warehouse <config_file>
```

### Config format (example)
```txt
# Customers
customer Maya soldier 4 2
customer David civilian 3 1

# Volunteers
volunteer Noya collector 2
volunteer Ibrahim collector 3 2
volunteer Din driver 13 4 2
volunteer Limor driver 8 3
```

## Project Structure
- `include/` – public headers
- `src/` – implementation files (parsing, domain logic, actions, main)
- `bin/` – build artifacts (executable and copied example config)
- `makefile` – build script used by the course toolchain

## Why this project
- Demonstrates designing a small but complete simulation engine with clear domain modeling.
- Emphasizes readable C++ and safe ownership semantics; passes `valgrind` with 0 leaks.

## Contributors
- Yuval Levy
- Tomer Faran
