# OpenAPI Collection Generator – Samples

Reference samples for the
[`openapi-collection-generator`](https://github.com/rspereiratech) ecosystem.
This repo demonstrates how to use each module — the **core** library, the
**Postman** generator, the **Insomnia** generator, and the **Maven plugin** —
against a small Pet Store OpenAPI 3.0 specification.

## What's in the box

```
.
├── pom.xml                               # Maven build, wires the Maven plugin
├── src/main/resources/
│   └── openapi.yaml                      # Sample Pet Store OpenAPI 3.0 spec
└── collections/                          # Output produced by the plugin
    ├── PetStore_postman.json
    ├── PetStore_insomnia.json
    └── PetStore.Production.environment.json
```

## Requirements

- Java 17+
- Maven 3.9+
- The other modules installed locally (run `mvn install` in each sibling repo,
  or use the top-level `build-all.sh`):
  - `openapi-collection-generator-parent`
  - `openapi-collection-generator-core`
  - `openapi-collection-generator-postman`
  - `openapi-collection-generator-insomnia`
  - `openapi-collection-generator-maven-plugin`

---

## 1. Using the Maven plugin (this project)

The simplest entry point. Add the plugin to your `pom.xml` and let it generate
both Postman and Insomnia collections during `generate-resources`:

```xml
<plugin>
    <groupId>com.github.rspereiratech</groupId>
    <artifactId>openapi-collection-generator-maven-plugin</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <executions>
        <execution>
            <id>generate-collections</id>
            <goals>
                <goal>generate</goal>
            </goals>
            <configuration>
                <formats>
                    <format>POSTMAN</format>
                    <format>INSOMNIA</format>
                </formats>
                <collectionName>PetStore</collectionName>
                <outputDirectory>${project.basedir}/collections</outputDirectory>
            </configuration>
        </execution>
    </executions>
</plugin>
```

Run it:

```bash
mvn generate-resources
```

Or invoke the goal directly from the command line:

```bash
mvn com.github.rspereiratech:openapi-collection-generator-maven-plugin:generate \
    -Dopenapi.spec=src/main/resources/openapi.yaml \
    -Dopenapi.format=INSOMNIA \
    -Dopenapi.outputDir=target/collections
```

Available configuration:

| Parameter         | Property                  | Default                                              |
|-------------------|---------------------------|------------------------------------------------------|
| `specFile`        | `openapi.spec`            | `${project.basedir}/src/main/resources/openapi.yaml` |
| `outputDirectory` | `openapi.outputDir`       | `${project.build.directory}/generated-collections`   |
| `formats`         | `openapi.format`          | `POSTMAN`                                            |
| `collectionName`  | `openapi.collectionName`  | API title from the spec                              |
| `baseUrl`         | `openapi.baseUrl`         | First server URL from the spec                       |

---

## 2. Using the core library programmatically

When you need to embed the generator in your own tool (CLI, Gradle plugin,
custom build step), depend on the core module directly:

```xml
<dependency>
    <groupId>com.github.rspereiratech</groupId>
    <artifactId>openapi-collection-generator-core</artifactId>
    <version>1.0.0-SNAPSHOT</version>
</dependency>
```

The single entry point is `CollectionGenerationOrchestrator`, which runs
the full pipeline (load → parse → generate → serialize → write):

```java
import com.github.rspereiratech.openapi.collection.generator.core.*;

GenerationRequest request = new GenerationRequest(
        new File("src/main/resources/openapi.yaml"),
        new File("build/collections"),
        List.of(CollectionFormat.POSTMAN, CollectionFormat.INSOMNIA),
        "PetStore"
);

CollectionGenerationOrchestrator orchestrator = /* wire components */;
List<GenerationResult> results = orchestrator.generate(request);

results.forEach(r -> System.out.println("Generated: " + r.collectionFile()));
```

Each `GenerationResult` exposes the produced collection file plus any
additional artifacts (e.g. environment files).

> **Tip:** when generating multiple formats in a single run, the file name
> pattern in `GenerationRequest` must include `{format}` so outputs don't
> overwrite each other.

---

## 3. Using the Postman generator directly

If you only care about Postman and want full control over wiring, use
`PostmanCollectionGenerator` from the Postman module:

```xml
<dependency>
    <groupId>com.github.rspereiratech</groupId>
    <artifactId>openapi-collection-generator-postman</artifactId>
    <version>1.0.0-SNAPSHOT</version>
</dependency>
```

```java
import com.github.rspereiratech.openapi.collection.generator.postman.generator.PostmanCollectionGenerator;

PostmanCollectionGenerator generator = new PostmanCollectionGenerator(
        operationGrouper,
        collectionSerializer,
        securityApplier,
        serverEnvironmentGenerator
);

String collectionJson = generator.generate(openApi, generationConfig);
List<AdditionalFile> environments =
        generator.generateAdditionalFiles(openApi, generationConfig);
```

Output:

- `<collection-name>.postman_collection.json` — Postman Collection v2.1.0
- `<server-name>.postman_environment.json` — one per OpenAPI server, with
  `baseUrl` plus any required security variables as placeholders

---

## 4. Using the Insomnia generator directly

Same idea, for Insomnia v4:

```xml
<dependency>
    <groupId>com.github.rspereiratech</groupId>
    <artifactId>openapi-collection-generator-insomnia</artifactId>
    <version>1.0.0-SNAPSHOT</version>
</dependency>
```

```java
import com.github.rspereiratech.openapi.collection.generator.insomnia.generator.InsomniaCollectionGenerator;

InsomniaCollectionGenerator generator = new InsomniaCollectionGenerator(
        idGenerator,
        insomniaRequestBuilder,
        collectionSerializer,
        /* ...remaining collaborators... */
);

String insomniaJson = generator.generate(openApi, generationConfig);
```

The resulting JSON can be imported directly via *Application → Preferences →
Data → Import Data* in Insomnia.

---

## Sample API

The included `openapi.yaml` describes a small **Pet Store** API with
endpoints for listing, creating, and retrieving pets — enough surface area
to exercise the most common request and response shapes (path parameters,
query parameters, JSON request bodies, multi-status responses).

## License

Released under the [MIT License](LICENSE).

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for setup and PR guidelines, and
[SECURITY.md](SECURITY.md) for how to report security issues.
