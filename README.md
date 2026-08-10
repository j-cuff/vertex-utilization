# vertex-utilization

`vertex-utilization.sh` generates a CSV report of Alloy imported cores, Pure
deployed cores, Kubernetes core hours (KCH), and total CPU cores from a Palette
or Palette VerteX instance.

## Requirements

- Bash
- `curl`
- `jq`
- A Palette or Palette VerteX API key

On macOS, install `jq` with Homebrew:

```bash
brew install jq
```

## Configure

From the `vertex-utilization` directory, copy the tracked template:

```bash
cp vertex.config.tmpl vertex.config
```

Edit `vertex.config` and set the following values:

- `PALETTE_API_URL` — Palette or VerteX API base URL. The script adds
  `https://` when no scheme is provided.
- `API_KEY` — API key used to query the dashboard projects endpoint.
- `PROJECT_UID` — optional project UID. Leave it empty to report every
  accessible project.
- `OUTPUT_FILE` — destination CSV path. The template uses
  `artifacts/utilization-report.csv`.

`vertex.config` is ignored by Git. Keep credentials out of
`vertex.config.tmpl`.

## Run

Generate a report using the local configuration:

```bash
./vertex-utilization.sh --config vertex.config
```

The default report is written to `artifacts/utilization-report.csv`. The script
creates the parent directory automatically, and `artifacts/` is ignored by Git.

Scope the report to one project or select another artifact filename:

```bash
./vertex-utilization.sh \
  --config vertex.config \
  --project-uid PROJECT_UID \
  --output artifacts/project-utilization.csv
```

## Configuration precedence

Command-line options override environment variables, which override values
loaded from the configuration file. Supported environment variables are:

- `PALETTE_API_URL`
- `API_KEY`
- `PROJECT_UID`
- `OUTPUT_FILE`
- `CONFIG_FILE` — configuration file path used instead of `--config`

For example:

```bash
CONFIG_FILE=vertex.config ./vertex-utilization.sh
```

## Options

```text
--config FILE       Load a KEY=VALUE configuration file
--api-url URL       Palette or VerteX API base URL
--api-key KEY       Palette or VerteX API key
--project-uid UID   Limit the report to one project
--output FILE       CSV path; defaults to artifacts/utilization-report.csv
--help              Show command help
```

Run `./vertex-utilization.sh --help` to display the command help.

## Report columns

Each cluster row contains the project name and tags, cluster name and UID,
Alloy imported cores and KCH, Pure deployed cores and KCH, and total cores. A
final `TOTAL` row sums the core and KCH columns across all returned clusters.
