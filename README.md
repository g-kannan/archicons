# archicons

Normalized SVG icon assets for CDN references in architecture diagrams.

## CDN usage

Icons are stored with lowercase kebab-case filenames under `icons/<category>/`.
Use the hosted registry for stable diagram references:

```text
https://archicons.oltpdba.workers.dev/registry.json
```

The architecture diagram generation guide is hosted alongside the registry:

```text
https://archicons.oltpdba.workers.dev/architecture_diagram_guide.md
```

Each registry entry includes a ready-to-use hosted icon URL:

```text
https://archicons.oltpdba.workers.dev/icons/aws/aws-lambda.svg
```

jsDelivr and raw GitHub URLs are also included in `registry.json` for tools
that prefer those sources.

## Registry

`registry.json` is the machine-readable index for architecture tooling. Each
entry includes:

- `id`: Stable lookup key, such as `aws.aws-lambda`
- `name`: Display name
- `provider`: Human-readable provider group
- `category`: Folder name under `icons/`
- `path`: Repository-relative SVG path
- `url`: Ready-to-use hosted SVG URL
- `cdn.jsdelivr`: jsDelivr CDN URL
- `cdn.hosted`: Hosted SVG URL on `archicons.oltpdba.workers.dev`
- `cdn.raw`: Raw GitHub URL

Registry URL:

```text
https://archicons.oltpdba.workers.dev/registry.json
```

Guide URL:

```text
https://archicons.oltpdba.workers.dev/architecture_diagram_guide.md
```

## Naming convention

- File and folder names use lowercase kebab-case.
- SVG files keep the `.svg` extension.
- Provider/category folders currently include `aws`, `azure`, `gcp`, and
  `data-platforms`.
- Product names in filenames avoid spaces, underscores, source-size suffixes,
  and vendor export prefixes.
