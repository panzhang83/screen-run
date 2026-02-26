---
name: screen-run
description: Run a python script in an existing screen session with logging
allowed-tools: Bash
argument-hint: [session-name] [conda-env] [log-file] [script.py and args...]
author: Pan Zhang panzhang@itp.ac.cn
---

The arguments are: $ARGUMENTS

Parse them as follows (in order): session-name, conda-env, log-file, and everything remaining is the full python command (script + args).

First, resolve the Python binary path for the given conda environment:

```
conda run -n <conda-env> which python
```

Then use the resolved path to launch the script in the screen session:

```
screen -S <session-name> -X screen bash -c "<resolved-python> -u <script.py and args...> 2>&1 | tee <log-file>; exec bash"
```

After running it, wait 8 seconds, then `tail -n 20 <log-file>` to check for startup errors (missing packages, CUDA OOM, etc). Report what you see.
