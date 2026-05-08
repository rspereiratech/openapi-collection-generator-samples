# OpenAPI Collection Generator – Samples

Sample project that demonstrates and validates the
[`openapi-collection-generator-maven-plugin`](https://github.com/rspereiratech/openapi-collection-generator-maven-plugin)
by generating Postman and Insomnia collections from an OpenAPI specification.

## What's inside

```
.
├── pom.xml                                  # Maven build wiring the plugin
├── src/main/resources/
│   └── openapi.yaml                         # Sample Pet Store OpenAPI 3.0 spec
└── collections/                             # Generated artifacts
    ├── PetStore_postman.json
    ├── PetStore_insomnia.json
    └── PetStore.Production.environment.json
```

## Requirements

- Java 17+
- Maven 3.8+
- The `openapi-collection-generator-maven-plugin` available in your local
  Maven repository (install it from its source repo with `mvn install`).

## Usage

Generate the collections by running:

```bash
mvn generate-resources
```

The plugin reads the OpenAPI spec at `src/main/resources/openapi.yaml` and
writes the resulting collections into the `collections/` directory.

## Configuration

The plugin is configured in [`pom.xml`](./pom.xml):

| Option            | Value                              |
|-------------------|------------------------------------|
| `formats`         | `POSTMAN`, `INSOMNIA`              |
| `collectionName`  | `PetStore`                         |
| `outputDirectory` | `${project.basedir}/collections`   |

Adjust these values to target different formats, names, or output
directories.

## Sample API

The included `openapi.yaml` describes a small **Pet Store** API with
endpoints for listing, creating, and retrieving pets — enough surface area
to exercise the most common request and response shapes.
