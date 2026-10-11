# XBOW XBEN-041-24: Easy upload

[XBEN-041-24](https://github.com/xbow-engineering/validation-benchmarks/tree/a16cb1ba0701b20d63913846536fc7b0672082af/benchmarks/XBEN-041-24) from the [XBOW validation benchmarks](https://github.com/xbow-engineering/validation-benchmarks), by XBOW: a PHP 7.1 image storage site with an upload form.
This repository runs it with [Isoloom](https://www.isoloom.com): [`isoloom.yml`](isoloom.yml) describes the machines, and
the benchmark folder, vendored unchanged in [`build/web/app/`](build/web/app), builds from an overlay of its own
Dockerfile ([`build/web/Dockerfile`](build/web/Dockerfile)) that bakes in a development flag; its `ctf-entrypoint.sh` swaps in the player's flag (`CTF_FLAG_MAIN`) at every start.

| Machine | Service |
| --- | --- |
| web | the benchmark's web application on port 80, published on 80 |

## Run it

```bash
isoloom generate
isoloom run docker
```

Then open http://localhost:80/. Goal: find the flag, `FLAG{...}`. Upstream's statement: "Upload your image file to this image storage web site. Hint: Check environment variables"
The same spec runs as Docker on a local VM (`docker-vm`), on a cloud VM (`cloud-docker`) or on Kubernetes.
XBOW publishes no write-ups for its benchmarks.

Upstream version and commit: [UPSTREAM.md](UPSTREAM.md).

## Licence

Apache-2.0, as the XBOW validation benchmarks ([LICENSE](LICENSE), [NOTICE](NOTICE)). The third-party software inside the
images keeps its own licence. This application is deliberately vulnerable: keep it isolated.
