# log-report

A Terminal-Bench 2 (Harbor) task: parse an Apache-style access log into a small JSON summary report.

## Overview
The agent reads `/app/access.log` and writes `/app/report.json` — a single JSON object with exactly three keys: `total_requests`, `unique_ips`, and `top_path`. See `instruction.md` for the full contract.

## Approach
Count each non-empty log line as one request, collect the first field of each line as the client IP into a set, extract the request path from the quoted request line, tally paths, and report the most-requested path (ties broken lexicographically). The reference solution is in `solution/`.

## Environment
`environment/Dockerfile` builds the single task image from an allowlisted base pinned by digest (`python:3.13-slim-bookworm`), with `pytest` and `pytest-json-ctrf` baked in at exact versions. Only the input `access.log` is copied into the image — no solution or test files.

## Verification
`tests/test.sh` runs `tests/test_outputs.py` and writes the reward to `/logs/verifier/reward.txt` plus a CTRF report to `/logs/verifier/ctrf.json`. The tests assert the real output values (6 requests, 3 unique IPs, top path `/index.html`), one test per success criterion — not merely that a file exists.

## Run locally
```
harbor run -p log-report -a oracle     # reference solution -> reward 1
harbor run -p log-report --agent nop   # no-op agent        -> reward 0
```
