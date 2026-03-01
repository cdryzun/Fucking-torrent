# Contributing

Thanks for your interest in improving this project!

## Adding a New Upstream Source

1. Fork the repo and create a branch.
2. Edit `.github/workflows/ci.yaml`.
3. Add a `fetch_trackers "<url>"` call in the **Fetch and aggregate tracker lists** step.
   - The URL must point to a plain-text file with one tracker URL per line (e.g. `udp://host:port/announce`).
4. Open a pull request and describe the new source (name, maintainer, update frequency).

## Reporting a False Positive

If a legitimate domain or IP is incorrectly included, open an issue with:
- The entry value
- Why it should not be blocked
- A reference link

## Reporting an Upstream Source Issue

If an upstream source is stale or unreliable, open an issue linking to the source and describing the problem.

## Code Style

- Shell scripts should pass `shellcheck`.
- Keep the workflow YAML minimal — no unnecessary steps or dependencies.
