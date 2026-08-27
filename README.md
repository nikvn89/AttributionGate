# AttributionGate

**GenLayer Intelligent Contract submission**

AttributionGate evaluates one narrow semantic property of a submitted statement:

```text
Does the statement itself place the declared author role in the position of
the party that will perform the described future action or produce the
described future result?
```

## Semantic outputs

```text
AUTHOR_COMMITMENT
NOT_AUTHOR_COMMITMENT
```

Malformed semantic output is normalized conservatively to:

```text
NOT_AUTHOR_COMMITMENT
```

## Deterministic consequence

```text
AUTHOR_COMMITMENT
-> owned_count += 1
-> recorded_count += 1

NOT_AUTHOR_COMMITMENT
-> owned_count unchanged
-> recorded_count += 1
```

`freeze_register` succeeds only when:

```text
owned_count >= required_commitments
```

Once frozen, the register is permanently closed to further statement submissions.

## Contract

```text
Name: AttributionGate
Network: StudioNet
Address: 0x51967e0b7583a939c8527370415EB7F6e83d267c
```

Explorer:

https://explorer-studio.genlayer.com/address/0x51967e0b7583a939c8527370415EB7F6e83d267c

## Multi-tenant design

Any wallet may create its own register.

Only the register creator may:

```text
submit_statement
freeze_register
```

There is no global administrator or deployer privilege.

## Important scope boundary

AttributionGate does not verify:

```text
- whether the sender wallet really has the declared real-world role
- legal enforceability
- promise strength
- whether a promise has a failure criterion
- whether a promise was or will be performed
- any external fact or web source
```

A FROZEN register means only that the contract recorded enough statements
classified as the declared author's own commitments.

## Runtime evidence

Observed on StudioNet:

```text
create_register                              PASS
hard case K1 -> NOT_AUTHOR_COMMITMENT        PASS
hard case K2 -> AUTHOR_COMMITMENT            PASS
quota -> QUOTA_MET                           PASS
freeze_register -> FROZEN                    PASS
submit_statement after FROZEN -> rollback    PASS
freeze_register twice -> rollback            PASS
non-creator submit_statement -> rollback     PASS
```

See `TESTING.md` for the exact observed values.
