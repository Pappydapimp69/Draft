# Findings Memo: `heartbeat.pyi`

**Date:** 2026-07-18
**Re:** Identification of `heartbeat.pyi` files found on the build environment
**Prepared by:** Claude Code (automated investigation)

---

To whom it may concern,

This letter documents the findings of an investigation into files named
`heartbeat.pyi` discovered on the session's build environment, prompted by a
request to run `python3 -m brain.heartbeat`.

## Summary

The `heartbeat.pyi` files are **unrelated to the Brain cognitive system**. They
are standard type-stub files bundled with the `pyright` type checker and belong
to the `pika` library. No Brain "heartbeat" script or module exists in this
environment.

## What was searched

- `find / -iname "*heartbeat*"` across the whole filesystem
- `grep -rli heartbeat` across `~/.brain`, `/home/user/Brain`, and the Draft
  repo's `.brain` directory
- Enumeration of every executable in the Brain install (`bin/`, `ci/`, `viz/`)

## Files found

Two identical copies, both inside pyright's bundled typeshed:

```
/root/.cache/uv/archive-v0/.../pyright/dist/dist/typeshed-fallback/stubs/pika/pika/heartbeat.pyi
/root/.local/share/uv/tools/pyright/lib/python3.11/site-packages/pyright/dist/dist/typeshed-fallback/stubs/pika/pika/heartbeat.pyi
```

(A third and fourth match — `/sys/.../tx_heartbeat_errors` — are Linux kernel
network-interface counters and are not files of interest.)

## What the file is

- A `.pyi` file is a **Python interface / type-stub file**: type annotations
  only, no runtime implementation. Type checkers such as pyright and mypy read
  these to verify code without executing it.
- This particular stub describes the `Heartbeat` class of **`pika`**, a Python
  client library for **RabbitMQ** (an AMQP message broker). AMQP heartbeats are
  periodic keep-alive frames that let broker and client detect a dropped
  connection.
- It arrived transitively as part of the `pyright` type-checker installation and
  has no connection to the project or to Brain.

## Conclusion

`python3 -m brain.heartbeat` cannot succeed here: `brain` is a standalone script
(`~/.brain/Brain/bin/brain`), not an importable Python package, and the installed
Brain version (`97a4185`) contains no `heartbeat` command, module, or script. The
only `heartbeat` artifacts present are the pika type stubs described above.

For a Brain health/liveness check, the equivalent commands are `brain status` and
`brain sync`.

Respectfully,

Claude Code
