# AttributionGate Runtime Evidence

This record separates deployment finality, semantic output, execution success, and verified post-state. A `null` write return value is not treated as proof by itself.

## Deployment identity

```text
Network: StudioNet (61999)
Contract: 0xE8D5F2807ff74aF68F2938b2c0d78b2911b69bB6
Deploy TX: 0xeb23aa9816d0d1045e0970cdab711cd147171d01391f01aee76a07746d1a3a0a
Class: AttributionGate
Version: 1.1
Source SHA-256: 50b56198975167d8ff11328be0f7f5cb01329ad48a884fa27ae5e6bc7c2314d9
Deployment result: GenVM SUCCESS / consensus Accepted
```

## Runtime actors and record

```text
Creator C:     0x6276095FAEA15108740445ff277fdA8c304657F4
Beneficiary B: 0xAD05365aFe0C2450d4FFBcdbE555b6E5fB7Dfa35
Register name: AttributionGate IC proof
Author role:   Sponsor
Required own commitments: 2
Register ID:   3dcaf56da223733a9d73cf17ef583861a8fa5dfeeca59cd8b7c326cc08906422
Statement limit: 4
```

## Completed runtime proof

| Step | Caller / method | Exact input or semantic result | Transaction | Verified post-state |
|---|---|---|---|---|
| Deploy | C / deploy | Exact frozen source | `0xeb23aa9816d0d1045e0970cdab711cd147171d01391f01aee76a07746d1a3a0a` | `SUCCESS`, Accepted |
| Create | C / `create_register` | Name `AttributionGate IC proof`, role `Sponsor`, beneficiary B, quota `2` | `0x5badde81cf6ada61fb23c0e09d83a97305f3fd27264be80ce1ed6b3f98a3833` | `OPEN`, recorded `0`, owned `0` |
| Negative semantic branch | C / `submit_statement` | `We understand the central laboratory will release the final assay results to investigators before database lock.` → `NOT_AUTHOR_COMMITMENT` | `0x43fdfb2a637875ba2efc36a5b6f8ff276679e5063fbf286b354a2967f9f17fb0` | `OPEN`, recorded `1`, owned `0` |
| Positive semantic branch 1 | C / `submit_statement` | `The Sponsor shall release the final assay results to investigators before database lock.` → `AUTHOR_COMMITMENT` | `0x742671ac668a5b013566d3a929fbdd9081a64f051697227e7ab406cbcfb0d8a7` | `OPEN`, recorded `2`, owned `1` |
| Positive semantic branch 2 | C / `submit_statement` | `We will release the final assay results to investigators before database lock.` → `AUTHOR_COMMITMENT` | `0x1c9496b628067084a5070cad0bc7729b1d3333b2bf731b660c4012c73fa2cd2e` | `QUOTA_MET`, recorded `3`, owned `2` |
| Freeze | C / `freeze_register` | Register ID | `0x413e0037b75bc9326a3a2292c80d4d6f2fdf704e7cc88d139f457706415cf9a4` | `FROZEN`, `frozen=true`, counters unchanged |
| Acknowledge | B / `acknowledge_register` | Register ID | `0xf33517fd5d6e6d7244b0487eb56ede97b7f6320de6cbaecb14f734d0247e90db` | `ACKNOWLEDGED`, `acknowledged=true`, counters unchanged |

All successful writes above showed `Result: SUCCESS` and consensus `Accepted`. The final accepted `get_register` response was:

```text
state: ACKNOWLEDGED
frozen: true
acknowledged: true
recorded_count: 3
owned_count: 2
required_commitments: 2
statement_limit: 4
creator: 0x6276095FAEA15108740445ff277fdA8c304657F4
beneficiary: 0xAD05365aFe0C2450d4FFBcdbE555b6E5fB7Dfa35
```

Validator executions cancelled after quorum in some transactions are not classified as failures: the overall consensus was Accepted, GenVM returned SUCCESS, the intended semantic result was present, and the accepted post-state matched the expected transition.

## Short reviewer path

1. Open the deployed contract in Explorer: https://explorer-studio.genlayer.com/address/0xE8D5F2807ff74aF68F2938b2c0d78b2911b69bB6
2. Verify the deploy transaction is successful and the contract address matches this document.
3. Open negative-branch transaction `0x43fdfb2a637875ba2efc36a5b6f8ff276679e5063fbf286b354a2967f9f17fb0`; verify `NOT_AUTHOR_COMMITMENT` and `SUCCESS`.
4. Open positive-branch transaction `0x1c9496b628067084a5070cad0bc7729b1d3333b2bf731b660c4012c73fa2cd2e`; verify `AUTHOR_COMMITMENT` and `SUCCESS`.
5. Load the contract in Studio and call `get_register` with `3dcaf56da223733a9d73cf17ef583861a8fa5dfeeca59cd8b7c326cc08906422`.
6. Confirm final state `ACKNOWLEDGED`, recorded `3`, owned `2`, quota `2`, `frozen=true`, and `acknowledged=true`.

The reviewer does not need to redeploy or use either runtime wallet for this read-only verification.

## Local source checks

The exact packaged source passed:

```text
python3 -m py_compile AttributionGate.py            PASS
SHA-256 parity                                      PASS
A1-A5 adversarial semantic fixture integrity        PASS
Python/TypeScript Unicode normalization parity      PASS
Static contract/project verification                PASS
```

The last three checks were executed in the full development workspace against the identical source bytes. They supplement, but do not replace, the StudioNet runtime proof above.
