# ADR-001: Engine-Independent Game Logic

**Status:** Accepted

**Decision Date:** 2026-11-22

## Context

Dungeon Game is built on top of MeshLibCore, a custom Java game engine.

As the project evolves from a game jam prototype into a collaborative and maintainable game project, a clear separation between gameplay logic and engine-specific functionality is required.

Gameplay mechanics should not be tightly coupled to rendering, graphical resources or engine-specific implementations.

This separation improves maintainability, testability and flexibility while allowing the game and engine to evolve independently.

## Decision

The package `dungeon.game` represents the engine-independent gameplay layer.

All functionality within this package must be independent of MeshLibCore and other engine-specific APIs.

The gameplay layer is responsible for:

- Game rules and mechanics.
- Gameplay state and state transitions.
- Player and enemy behavior.
- Combat and damage calculations.
- Items and gameplay effects.
- Game-specific data and logic.

Engine-specific functionality, including rendering, input handling, resource management and graphical operations, must remain outside this package.

Communication between gameplay logic and engine-specific systems should happen through clearly defined interfaces or explicit integration points.

## Architectural Principles

1. Gameplay logic must not depend on engine-specific implementations.
2. Rendering must not contain gameplay rules.
3. Engine functionality should remain reusable independently of Dungeon Game.
4. Dependencies should point towards the gameplay abstractions rather than introducing unnecessary coupling.
5. New functionality should respect this separation from the beginning.

## Consequences

**Positive:**
- Gameplay logic becomes easier to test independently.
- Engine and game development can evolve separately.
- Systems become easier to understand and maintain.
- Future changes to rendering or engine implementations have less impact on gameplay.

**Trade-offs:**
- Some communication requires additional interfaces or integration code.
- Existing classes may need to be refactored.
- Responsibilities must be defined more explicitly.

## Implementation Strategy

The separation will be introduced incrementally.

Existing functionality will be reviewed and refactored without requiring a complete rewrite of the project.

The package structure and dependency rules will serve as the foundation for future development.
