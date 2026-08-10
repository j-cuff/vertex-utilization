# vertex-utilization

Generate a CSV report of Alloy imported cores, Pure deployed cores, KCH, and
total CPU cores from a Palette or Palette VerteX instance.

## Configuration

Create the local configuration from the tracked template:

```bash
cp vertex.config.tmpl vertex.config
```

Edit `vertex.config` and set:

- `PALETTE_API_URL` — Palette or VerteX API base URL.
- `API_KEY` — API key used to query the dashboard projects endpoint.
- `PROJECT_UID` — optional project UID; leave empty to report all projects.
- `OUTPUT_FILE` — destination CSV filename. It defaults to
  `artifacts/utilization-report.csv`.

`vertex.config` is ignored by Git. Do not put credentials in
`vertex.config.tmpl`.

The script creates the output file's parent directory automatically. The
default `artifacts/` directory is ignored by Git.

## Usage

```bash
./vertex-utilization.sh --config vertex.config
```

Command-line options override values loaded from the configuration file:

```bash
./vertex-utilization.sh \
  --config vertex.config \
  --project-uid PROJECT_UID \
  --output utilization-report.csv
```

Run `./vertex-utilization.sh --help` for all options.
