# Open Growing Protocol (OGP) Monorepo

This repository serves as the central hub for the Open Growing Protocol (OGP), a standardized protocol for autonomous plant cultivation systems. It provides a unified view of the OGP specifications, schemas, and related resources.

## Repository Structure

The repository is organized as a monorepo, with the following main directories:

-   `.clinerules`: Contains the Cline rules for this repository.
-   `.gitmodules`: Defines the submodules used in this repository.
-   `memory-bank/`: Contains the memory bank files for the Cline agent.
-   `profile/`: Contains the files for the organization profile on GitHub.
-   `schemas/`: Contains the OGSP schema definitions.
-   `specifications/`: Contains the OGSP specification documents.

## Submodule Details

This repository utilizes Git submodules to manage external dependencies:

-   ### [specifications](https://github.com/opengrowingalliance/specifications)

    Contains the OGSP specification documents, which define the protocol's architecture, components, and interfaces.

-   ### [schemas](https://github.com/opengrowingalliance/schemas)

    Contains the OGSP schema definitions, which define the data structures used in the protocol.

## Specifications Guide

The OGSP specifications are organized into different versions, each representing a significant evolution of the protocol:

-   ### v1.0

    The initial version of the OGSP specification, which established the core concepts and requirements.

    -   [ogsp-spec.md](specifications/v1.0/ogsp-spec.md)

-   ### v1.1

    An expansion of v1.0, providing more detailed implementation guidance.

    -   [ogsp-spec.md](specifications/v1.1/ogsp-spec.md)
    -   [appendices.md](specifications/v1.1/appendices.md)

-   ### v2.0

    A significant evolution of the OGSP approach, adopting a modular specification structure.

    -   [ogsp-root-spec.md](specifications/v2.0/ogsp-root-spec.md)
    -   [core/ogsp-core-spec.md](specifications/v2.0/core/ogsp-core-spec.md)
    -   [hardware/ogsp-hardware-spec.md](specifications/v2.0/hardware/ogsp-hardware-spec.md)
    -   [recipe/ogsp-recipe-spec.md](specifications/v2.0/recipe/ogsp-recipe-spec.md)
    -   [recipe/ogsp-recipe-spec-revised.md](specifications/v2.0/recipe/ogsp-recipe-spec-revised.md)

## Schemas Guide

The OGSP schemas define the data structures used in the protocol:

-   ### device.schema.json

    Defines the schema for OGSP hardware devices, including sensors, actuators, and controllers.

-   ### recipe.schema.json

    Defines the schema for OGSP growth recipes, which specify the steps and parameters for cultivating plants.

## Getting Started

To get started with OGSP development:

1.  Clone this repository:

    ```bash
    git clone https://github.com/opengrowingalliance/.github
    cd .github
    ```

2.  Initialize the submodules:

    ```bash
    git submodule init
    git submodule update
    ```

3.  Explore the specifications and schemas to understand the protocol's architecture and data structures.

## Contribution Guidelines

Please refer to the [OGSP Contribution Guidelines](https://opengrowingalliance.solar/contribute) for information on how to contribute to the project.

## License

The OGSP is released under the [Apache 2.0 License](LICENSE).
