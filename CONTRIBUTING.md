# Contributing

Thanks for your interest in improving the **OpenAPI Collection Generator – Samples** project. This repo exists to demonstrate and validate the
[`openapi-collection-generator`](https://github.com/rspereiratech) ecosystem against a real OpenAPI spec, so contributions should keep that goal in mind: small, illustrative, and easy to run.

## Ways to contribute

- Fix bugs in the sample spec or the generated output.
- Add new sample endpoints that exercise OpenAPI features not yet covered (e.g. additional security schemes, polymorphic schemas, file uploads).
- Improve the README or other documentation.
- Report issues you hit while running the samples.

For changes to the generator itself (core, Postman, Insomnia, Maven plugin), please open the PR against the corresponding repo, not this one.

## Getting set up

Prerequisites:

- Java 17+
- Maven 3.9+
- The sibling modules installed locally:
  - `openapi-collection-generator-parent`
  - `openapi-collection-generator-core`
  - `openapi-collection-generator-postman`
  - `openapi-collection-generator-insomnia`
  - `openapi-collection-generator-maven-plugin`

Install them via `mvn install` in each repo, or run the top-level `build-all.sh`.

Then, from this repo:

```bash
mvn generate-resources
```

The generated collections land in `collections/`. Open them in Postman or Insomnia to verify they import cleanly.

## Making changes

1. Fork the repo and create a topic branch off `master`.
2. Keep changes focused — one logical change per PR.
3. If you change `src/main/resources/openapi.yaml`, regenerate the collections and commit the updated files in `collections/` so the repo stays in sync.
4. Validate locally:
   ```bash
   mvn clean generate-resources
   ```
5. Import the generated collections into Postman and Insomnia to confirm they still load without errors.

## Commit messages

Follow the existing style (see `git log`): a short imperative subject line, no trailing period, optional body for the *why*. Example:

```
Add multipart upload endpoint to Pet Store sample
```

## Pull requests

- Describe what changed and why.
- Mention any sibling-repo changes the PR depends on.
- Note manual verification steps (e.g. "imported into Postman 11.x, Insomnia 9.x").

## Reporting issues

Open an issue with:

- The version of each sibling module you have installed.
- The exact Maven command you ran.
- The full output, including any stack trace.
- The OpenAPI spec snippet that triggered the problem, if applicable.

## License

By contributing, you agree that your contributions will be licensed under the same license as the parent project.
