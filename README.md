# About logup

Logup is a UNIX-style command that can be used to pipe stdout logs to a location on disk or in the cloud without the need of an agent, logrotate, systemd or other configuration files.

Logup is resilient: it does buffering to temp files to prevent the application from ever blocking when writing to stdout. (Not implemented yet)

Logup is transparent: it passes through the original stdout without any additional info or error messages.

## Use cases

Upload to AWS Logs:

```bash
# environment with region and credentials
$ echo foo | logup --aws --aws-log-group-name '/test/foo'
foo
```

Upload to NewRelic:

```bash
$ export NEW_RELIC_API_KEY = "..."
$ echo foo | logup --newrelic --newrelic-region EU
foo
```

Pipe stdout to disk files with log rotation, without the need to set up logrotate. (Not implemented yet)

## Installation ![](https://github.com/lucabrunox/logup/actions/workflows/ci.yml/badge.svg)

To install the latest release in ~/.cargo/bin:

```bash
cargo install logup
```

## Command line usage

```
Usage: logup [OPTIONS] [INPUT_FILE]

Arguments:
  [INPUT_FILE]  Read logs from a file instead of stdin

Options:
      --aws
          Enable uploading logs to AWS Logs
      --aws-log-group-name <AWS_LOG_GROUP_NAME>
          [env: AWS_LOG_GROUP_NAME]
      --aws-log-stream-name <AWS_LOG_STREAM_NAME>
          Log stream name [default: hostname] [env: AWS_LOG_STREAM_NAME]
      --newrelic
          Enable uploading logs to NewRelic
      --newrelic-region <NEW_RELIC_REGION>
          [env: NEW_RELIC_REGION] [possible values: US, EU]
      --newrelic-api-key <NEW_RELIC_API_KEY>
          [env: NEW_RELIC_API_KEY]
      --max-line-size <MAX_LINE_SIZE>
          Force flush without newline beyond the given size [default: 1000000]
      --max-memory-items <MAX_MEMORY_ITEMS>
          Max logs to keep in memory before dropping the incoming ones [default: 1000]
      --max-retries <MAX_RETRIES>
          Max retries before dropping a log [default: 100]
  -h, --help
          Print help
  -V, --version
          Print version
```

## Roadmap

- [X] Send logs to AWS Logs
- [X] Buffering in-memory
- [X] Splitting by lines
- [X] Read from file instead of just stdout
- [ ] Make it easy to wrap a Docker entrypoint
- [ ] Buffering on-disk
- [ ] Output to disk files with log rotation
- [ ] Compression
- [ ] Logging of logup itself to disk
- [ ] Expose Prometheus endpoint of logup itself
- [ ] Distributions
  - [X] Cargo
  - [ ] Tar
  - [ ] Deb
  - [ ] Rpm
- [ ] Support more outputs
  - [ ] Cloud providers
  - [ ] Syslog
  - [ ] OTLP

## License

Logup is licensed under the GPLv3: https://www.gnu.org/licenses/gpl-3.0.html#license-text

All contributions are welcome.
