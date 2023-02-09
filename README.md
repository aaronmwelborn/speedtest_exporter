### Project Forked from [danopstech/speedtest_exporter|(https://github.com/danopstech/speedtest_exporter)]

<p align=center>
  <img alt=logo src="https://github.com/aaronmwelborn/speedtest_exporter/raw/main/.docs/assets/logo.jpg" height=150 />
  <h3 align=center>Speedtest Prometheus Exporter</h3>
</p>

---
A [Speedtest](https://www.speedtest.net) exporter for Prometheus.

[![goreleaser](https://github.com/aaronmwelborn/speedtest_exporter/actions/workflows/release.yaml/badge.svg)](https://github.com/aaronmwelborn/speedtest_exporter/actions/workflows/release.yaml)
[![build](https://github.com/aaronmwelborn/speedtest_exporter/actions/workflows/build.yaml/badge.svg)](https://github.com/aaronmwelborn/speedtest_exporter/actions/workflows/build.yaml)
[![License](https://img.shields.io/github/license/aaronmwelborn/speedtest_exporter)](/LICENSE)
[![Release](https://img.shields.io/github/release/aaronmwelborn/speedtest_exporter.svg)](https://github.com/aaronmwelborn/speedtest_exporter/releases/latest)
[![Docker](https://img.shields.io/docker/pulls/aaronmwelborn/speedtest_exporter)](https://hub.docker.com/r/aaronmwelborn/speedtest_exporter)
![GitHub go.mod Go version](https://img.shields.io/github/go-mod/go-version/aaronmwelborn/speedtest_exporter)
![os/arch](https://img.shields.io/badge/os%2Farch-amd64-yellow)
![os/arch](https://img.shields.io/badge/os%2Farch-arm64-yellow)
![os/arch](https://img.shields.io/badge/os%2Farch-armv7-yellow)
[![Go Report Card](https://goreportcard.com/badge/github.com/aaronmwelborn/speedtest_exporter)](https://goreportcard.com/report/github.com/aaronmwelborn/speedtest_exporter)

## Usage:

### Flags

`speedtest_exporter` is configured by optional command line flags

```bash
$ ./speedtest_exporter --help
Usage of speedtest_exporter
  -port string
        listening port to expose metrics on (default "9877")
  -server_fallback
        If the serverID given is not available, should we fallback to closest available server
  -server_id int
        Speedtest.net server ID to run test against, -1 will pick the closest server to your location (default -1)

```

### Binaries

For pre-built binaries please take a look at the [releases](https://github.com/aaronmwelborn/speedtest_exporter/releases).

```bash
./speedtest_exporter [flags]
```

### Docker

Docker Images can be found at [GitHub Container Registry](https://github.com/orgs/aaronmwelborn/packages/container/package/speedtest_exporter) & [Dockerhub](https://hub.docker.com/r/aaronmwelborn/speedtest_exporter).

Example:
```bash
docker pull ghcr.io/aaronmwelborn/speedtest_exporter:latest

docker run \
  -p 9877:9877 \
  ghcr.io/aaronmwelborn/speedtest_exporter:latest [flags]
```

### Setup Prometheus to scrape `speedtest_exporter`

Configure [Prometheus](https://prometheus.io/) to scrape metrics from localhost:9877/metrics

This exporter locks (one concurrent scrape at a time) as it conducts the speedtest when scraped, **remember set scrape interval, and scrap timeout** accordingly as per example.

```yaml
...
scrape_configs
    - job_name: speedtest
      scrape_interval: 60m
      scrape_timeout:  60s
      static_configs:
        - targets: ['localhost:9877']
...
```

## Exported Metrics:

```
# HELP speedtest_download_speed_Bps Last download speedtest result
# TYPE speedtest_download_speed_Bps gauge
# HELP speedtest_latency_seconds Measured latency on last speed test
# TYPE speedtest_latency_seconds gauge
# HELP speedtest_scrape_duration_seconds Time to preform last speed test
# TYPE speedtest_scrape_duration_seconds gauge
# HELP speedtest_up Was the last speedtest successful.
# TYPE speedtest_up gauge
# HELP speedtest_upload_speed_Bps Last upload speedtest result
# TYPE speedtest_upload_speed_Bps gauge
```
## Example Grafana Dashboard:

https://grafana.com/grafana/dashboards/14336

<p align="center">
	<img src="https://github.com/aaronmwelborn/speedtest_exporter/raw/main/.docs/assets/screenshot.jpg" width="95%">
</p>
