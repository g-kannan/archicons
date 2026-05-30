# archicons

Normalized SVG icon assets for CDN references in architecture diagrams.

## CDN usage

Icons are stored with lowercase kebab-case filenames under `icons/<category>/`.
Use jsDelivr for stable diagram references:

```text
https://cdn.jsdelivr.net/gh/g-kannan/archicons@main/icons/<category>/<icon>.svg
```

Example:

```text
https://cdn.jsdelivr.net/gh/g-kannan/archicons@main/icons/aws/aws-lambda.svg
```

Raw GitHub URLs are also included in `registry.json` for tools that prefer
`raw.githubusercontent.com`.

## Registry

`registry.json` is the machine-readable index for architecture tooling. Each
entry includes:

- `id`: Stable lookup key, such as `aws.aws-lambda`
- `name`: Display name
- `provider`: Human-readable provider group
- `category`: Folder name under `icons/`
- `path`: Repository-relative SVG path
- `cdn.jsdelivr`: jsDelivr CDN URL
- `cdn.raw`: Raw GitHub URL

Registry URL:

```text
https://cdn.jsdelivr.net/gh/g-kannan/archicons@main/registry.json
```

## Naming convention

- File and folder names use lowercase kebab-case.
- SVG files keep the `.svg` extension.
- Provider/category folders currently include `aws`, `azure`, `gcp`, and
  `data-platforms`.
- Product names in filenames avoid spaces, underscores, source-size suffixes,
  and vendor export prefixes.
