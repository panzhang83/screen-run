# screen-run

A Claude Code skill that runs a Python script inside an existing [GNU Screen](https://www.gnu.org/software/screen/) session with output logging.

Useful for launching long-running jobs (e.g. ML training) in persistent terminal sessions, capturing full logs, and getting an immediate sanity check on startup errors.

## How it works

1. Resolves the Python binary from the given Conda environment
2. Injects a new window into the target screen session running the script with unbuffered output piped to a log file
3. Waits 8 seconds, then tails the log to surface any startup errors (missing packages, CUDA OOM, etc.)

## Installation

Copy `SKILL.md` into your Claude Code skills directory (typically `~/.claude/skills/`).

## Usage

```
/screen-run [session-name] [conda-env] [log-file] [script.py and args...]
```

### Example

```
/screen-run training myenv /tmp/train.log train.py --epochs 100 --lr 0.001
```

This will:
- Attach to the screen session named `training`
- Use the Python binary from the `myenv` Conda environment
- Log all output (stdout + stderr) to `/tmp/train.log`
- Report the first few lines of output after startup

## Requirements

- [GNU Screen](https://www.gnu.org/software/screen/)
- [Conda](https://docs.conda.io/)

## License

Apache 2.0
