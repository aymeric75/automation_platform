# Architecture

## Direction

The system should eventually support this generic flow:

Input -> processing / extraction -> business rules -> validation -> action -> audit

Client-specific behavior should live behind configuration or adapters whenever practical.

## Rule

Do not add architectural layers, services or abstractions until a concrete use case requires them.
