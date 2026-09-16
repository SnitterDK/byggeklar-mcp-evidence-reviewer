> **Source package:** Download `ByggeKlar-MCP-professional-source-2026-09-16.zip` from this repository and extract it first. Run the standalone quick-start commands below from the extracted directory containing `http_server.py`. The historical `devpost/byggeklar-mcp/` paths refer to the original workspace; omit that prefix in the extracted standalone package. No paid account or API key is required.

# ByggeKlar · MCP Evidence Reviewer

**Clearer preparation. Better questions. Human decisions.**

A local preparation workspace for homeowners and small contractors. Describe a
building project, identify missing evidence, and export structured findings for
human review. This is not a permit service or a legal opinion.

## Standalone quick start

From this package directory, with Python 3.10 or later:

```sh
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements-http.txt
python http_server.py
```

In another terminal in the same directory:

```sh
source .venv/bin/activate
python demo.py --http
```

Open http://127.0.0.1:8765/. Review an incomplete case, fill sample measurements,
check the document flags, then review again and download the JSON result.
The flags are owner declarations, not verified documents. A complete checklist
never grants a building permit.

```text
Browser form → local backend → Streamable HTTP MCP → checklist engine
             ← structured findings and limitations ←
```

Run `python -m unittest discover -s . -v` for the 13 unit tests. With both servers
running, use `python verify_http.py` and `python verify_demo_http.py` to verify
the real local HTTP chain. No paid account or API key is required.

## Implementation notes and verification history

12 September: rules recheck identifies Streamable HTTP as required for the
MCP submission path. The stdio demo alone is insufficient. Added http_server.py
using the official SDK transport, sharing the existing review function.
The new entry point was runtime-tested on 12 September with the official SDK
1.30.0 client over actual loopback HTTP: protocol 2025-11-25, discovery, tool call,
invalid negative measurement rejection and ping passed. No public hosting,
Alexa connection or finished competition eligibility is claimed.

With Python 3.10+ and a dedicated virtual environment:

    pip install -r devpost/byggeklar-mcp/requirements-http.txt
    python devpost/byggeklar-mcp/http_server.py

Intended local endpoint: http://127.0.0.1:8766/mcp (not the browser demo on 8765).
Reproduce the check with `python devpost/byggeklar-mcp/verify_http.py` while the
server runs. Start the browser demonstrator with `python devpost/byggeklar-mcp/demo.py --http`
in a second terminal, then run `python devpost/byggeklar-mcp/verify_demo_http.py`.
On 16 September this actual HTTP chain passed missing-document, completed-checklist
without permit approval, and negative-input checks. Without --http the original
stdio mode remains available. No external hosting or authentication is configured.

New work started 7 September 2026 for the Amazon Developer Alexa+ track.
Reuses the existing ByggeKlar checklist engine without modifying it. The new
component exposes that deterministic engine through an MCP stdio tool. It is
not yet an Alexa-connected skill or a hosted server, and has no voice UI yet.
A local browser form is now available via `python3 devpost/byggeklar-mcp/demo.py`
at http://127.0.0.1:8765/. It calls the real MCP subprocess for each review,
displays findings and exports review JSON. It is not a simulated chat or voice agent.
On 16 September the page passed browser interaction checks for incomplete and
complete checklists, including clearing stale results after inputs change.
Video recording and publication remain outstanding.

Run from the workspace root:

    python3 devpost/byggeklar-mcp/server.py
    python3 -m unittest discover -s devpost/byggeklar-mcp -v

Use a local MCP client with command `python3` and the absolute path to server.py
as its argument. The reused engine and its license are included in this folder.
No model account, key, network, payment or storage is used by this server.
It provides initialize, initialized notification, ping, tools/list and tools/call
with protocol version 2025-11-25. The official MCP Python SDK 1.30.0 client
successfully negotiated and called it on 7 September 2026; saved evidence is in
evidence/sdk-check-2026-09-07.json. This is not full conformance or Alexa validation.

Reproduce the independent SDK-client check in a Python 3.10+ virtual environment:

    pip install -r devpost/byggeklar-mcp/requirements-test.txt
    python devpost/byggeklar-mcp/verify_sdk.py

The check uses synthetic local data and no model service or API key.

Tool input uses municipality, project type, three measurements and three
document flags. Omit precise address and personal information. Missing measurements
are null. A claimed document is not independently verified. A complete checklist
is NOT a permit or a legal eligibility determination. Nothing is submitted or mailed.

Before competition submission: test an actual agent/client connection, build and
record the user experience, package reusable engine and license in a public repo,
provide demo and disclose which work predates the competition. Do not claim Alexa
runtime or finished competition eligibility from these local tests alone.

Sources checked 7 September 2026:
- https://amazonappdev2026.devpost.com/rules
- https://modelcontextprotocol.io/specification/2025-11-25/basic/lifecycle
- https://modelcontextprotocol.io/specification/2025-11-25/server/tools
