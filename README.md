# go-temporal-geolocation-example

A simple [Temporal](https://temporal.io) workflow written in Go that looks up the public IP address of the machine running the worker, then resolves it to a city/region/country using a free geolocation API.

## What it does

The `GetAddressFromIP` workflow runs two activities in sequence:

1. **GetIP** — fetches the worker machine's public IP via [icanhazip.com](https://icanhazip.com)
2. **GetLocationInfo** — resolves that IP to a location via [ip-api.com](http://ip-api.com)

The workflow returns a greeting string, e.g.:

```
Hello, Alice. Your IP is 203.0.113.42 and your location is San Francisco, California, United States
```

## Prerequisites

- [Go 1.24+](https://go.dev/dl/)
- A running Temporal server — the quickest way is via the [Temporal CLI](https://docs.temporal.io/cli):

  ```sh
  temporal server start-dev
  ```

## Project structure

```
.
├── activities.go        # GetIP and GetLocationInfo activity implementations
├── workflows.go         # GetAddressFromIP workflow definition
├── shared.go            # Shared constants (task queue name)
├── activities_test.go   # Unit tests for activities
├── workflows_test.go    # Unit test for the workflow
├── worker/
│   └── main.go          # Starts the Temporal worker
└── client/
    └── main.go          # Triggers the workflow and prints the result
```

## Running the example

**1. Start the worker** (in one terminal):

```sh
go run ./worker
```

**2. Trigger the workflow** (in another terminal), passing your name as an argument:

```sh
go run ./client -- YourName
```

The client will print the result once the workflow completes.

## Running tests

```sh
go test ./...
```

Tests use Temporal's test suite and mock the HTTP client, so no live server or network access is required.
