# catincloud-labs

Infrastructure and data tooling, run as a small self-hosted data-operations
platform and operated as a thing that is running rather than a thing that is
being built. Building happens, but as the bounded exception. This page
describes the shape of that platform and the mechanisms it relies on. It does
not describe how well they perform, on purpose; the reason is at the bottom.

If you read one thing, read the package at the end. It runs from a clean
checkout with no private access.

## The shape

A fleet of services on peer hosts joined by a private overlay network. The
founding thesis is that every host is the same class of node and can carry any
role the others can; no role may be pinned to a host by accident. "Distributed"
is treated as a measured property of the running system, not of the diagram.

The components, by role:

- A coordination store: the shared state that leases, tenures and publication
  pointers are read from and compared-and-set against.
- Role holders: services that acquire a role by lease, renew it while healthy,
  and are refused the moment they act on a role they no longer hold.
- A message layer: framed traffic between nodes, with the frame checked before
  the payload is looked at.
- A publication path: derived state written as a content-addressed artefact
  behind a versioned pointer.
- A spend ledger: per-principal metering of paid external calls, in front of
  every billed request.
- An orchestrator and asset graph: the data work itself, declared as assets,
  scheduled and observed.
- A declarations repository: the platform's own configuration, reachable from a
  host only through a pull request a human merges.
- Alerting with dead-man's switches, so that a total outage does not have to
  send its own alert.

If that reads like a ground segment rather than an application, that is the
intended comparison: orchestration, telemetry, limits, redundancy and
fault-detection-and-recovery, not flight software.

## How it behaves

- A role is a lease on the coordination store; the store's contract is read and
  compare-and-set, backed by Postgres, and a conformance suite tells a real
  backend from ones written to be wrong on purpose.
- A holder renews its lease; if it dies, the lease lapses and a peer claims it
  inside a window that was derived and written down before the run, and a
  commanded handover (release while healthy) is drilled on a cadence.
- Every routed claim carries the tenure it was made under and is checked where
  it is served, so a host that has lost a role is fenced at its next action,
  not at its next heartbeat.
- A message between nodes is a fixed binary header (version, kind, sequence,
  length, checksum) around a payload, the header is checked first, and a suite
  of frames built wrong on purpose is part of the tests.
- Derived state is published as a content-addressed artefact plus a versioned
  pointer; a publisher advancing from a generation the store has already left
  is refused, and readers re-hash before replacing what they hold.
- Spend on paid APIs fails closed: if the per-principal ledger cannot be
  reached, billed calls are refused rather than priced at zero.
- The only write path from the platform back to its own declarations is a pull
  request a human merges; a production host is read from and is never granted a
  way out, so a proposal born on a host becomes a pull request nobody on that
  host could open.
- Every control is inventoried three ways: what would tell us if it silently
  stopped working, whether anyone has exercised it against the real thing, and
  whose defect a finding is.
- Silence is treated as blindness until an alert has fired on an injected known
  negative.

## How it is governed

- A decision is an immutable dated record, superseded rather than edited; the
  record of having changed one's mind is the valuable part.
- A record states what may be built. "Accepted" authorises a decision, not a
  build; a design document describes and never decides.
- A published claim is one a stranger can reproduce from a clean checkout, so
  measurements stay private and the mechanism that would fail is what ships.
- A control that must hold in every repository is organisation policy keyed to
  a repository property, so a new repository is gated at creation rather than
  by remembering to configure it.
- A third-party action is pinned to a commit, and the pin has a named mover.
- A credential is scoped by the artefact it reaches, and reachability is a
  computed closure over what it can touch, not a listing someone maintains.
- The unit of work at cruise is the cycle: a dependency month, an upstream
  release, a scheduled move, the scheduled readout.

## What you can run

The public artefact is `dagster-dex`: a Dagster asset graph read as a project
format for [dex](https://github.com/exmergo/dex), an engine that reads a
project's declared structure and reports where the warehouse has drifted from
it. Apache-2.0, on PyPI.

    pip install dagster-dex

Source, issues and the design argument are at
<https://github.com/catincloud-labs/dagster-dex>. Every command in that README
runs from a clean checkout with no private access; if one does not, that is a
defect, and the README is where to report it.

## Who

David Anaya. <https://github.com/catincloudlabs>

---

This page describes shape and mechanism only. It carries no measurement,
no count, no identifier and no link into private work, on purpose: a figure a
reader cannot reproduce is a claim they are being asked to take on trust, and
the mechanism is the part that would be interesting if it failed.
