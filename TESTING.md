# AttributionGate — StudioNet Runtime Testing

Contract:

```text
0x51967e0b7583a939c8527370415EB7F6e83d267c
```

Explorer:

https://explorer-studio.genlayer.com/address/0x51967e0b7583a939c8527370415EB7F6e83d267c

Creator wallet:

```text
0x6276095FAEA15108740445ff277fdA8c304657F4
```

Non-creator wallet used for authorization testing:

```text
0x0a9Fa0b9a0B42981e43A8fb9A1D710E48cc7e095
```

This document records only outcomes actually observed on StudioNet.

## T1 — Create register

```text
name:
Migration commitments

author_role_label:
Platform Team

required_commitments:
1
```

Observed:

```text
SUCCESS
```

Register ID:

```text
b8a45540bef1fbd22b48fd4dc139770928367294c440fffae5b106e3de5fe799
```

Result: `PASS`

## T2 — Kill-concept K1

Statement:

```text
The team expects the migration plan to be published before production cutover.
```

Observed:

```text
owned_count = 0
recorded_count = 1
state = OPEN
frozen = false
```

Matches:

```text
NOT_AUTHOR_COMMITMENT
```

Result: `PASS`

## T3 — Kill-concept K2

Statement:

```text
The undersigned undertakes to publish the migration plan before cutover.
```

Observed semantic output:

```text
AUTHOR_COMMITMENT
```

Observed state:

```text
owned_count = 1
recorded_count = 2
required_commitments = 1
state = QUOTA_MET
frozen = false
```

Result: `PASS`

## T4 — Freeze after quota met

Observed state:

```text
owned_count = 1
recorded_count = 2
frozen = true
state = FROZEN
```

Result: `PASS`

## T5 — Submit after frozen

Observed rollback:

```text
Register is frozen
```

Result: `PASS`

## T6 — Freeze twice

Observed rollback:

```text
Register is already frozen
```

Result: `PASS`

## T7 — Non-creator submit denied

Observed rollback:

```text
Only register creator may submit statements
```

Result: `PASS`

## Runtime summary

```text
create register                               PASS
K1 -> NOT_AUTHOR_COMMITMENT                   PASS
K2 -> AUTHOR_COMMITMENT                       PASS
quota -> QUOTA_MET                            PASS
freeze -> FROZEN                              PASS
submit after frozen rejected                  PASS
second freeze rejected                        PASS
non-creator submit rejected                   PASS
```

## Not claimed PASS

The following defensive cases were not separately runtime-tested in this flow:

```text
empty statement
801-character statement
exact replay
non-creator freeze
quota 0 or 11
statement 21
reserved-token injection
role-label line-break rejection
same register name from another wallet
long >150-character submit_statement calldata path
```
