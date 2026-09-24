# AttributionGate

AttributionGate is a GenLayer Intelligent Contract that distinguishes a party's own future commitment from quotations, reports, expectations, and statements about another actor. Only statements classified as `AUTHOR_COMMITMENT` advance the immutable quota. Once the quota is met, the register creator can freeze the record and the designated beneficiary can acknowledge it.

## Deployed contract

- Network: StudioNet (chain ID 61999)
- Contract class: `AttributionGate`
- Version: `1.1`
- Contract address: `0xE8D5F2807ff74aF68F2938b2c0d78b2911b69bB6`
- Deploy transaction: `0xeb23aa9816d0d1045e0970cdab711cd147171d01391f01aee76a07746d1a3a0a`
- Explorer: https://explorer-studio.genlayer.com/address/0xE8D5F2807ff74aF68F2938b2c0d78b2911b69bB6
- Source file: `AttributionGate.py`
- Source SHA-256: `50b56198975167d8ff11328be0f7f5cb01329ad48a884fa27ae5e6bc7c2314d9`
- Source size: 23,594 bytes / 758 lines

The deployment returned `GenVM SUCCESS`, consensus `Accepted`, and `get_config()` reports `contract_name=AttributionGate` and `version=1.1`. The internal `project_name=OwnThePromise` is retained because this Intelligent Contract uses the exact frozen source bytes that were runtime-tested; the contract class and submission title remain AttributionGate.

## How it works

1. A creator opens a register with a declared author role, beneficiary, and required number of own commitments.
2. The creator submits statements. Each statement receives one narrow semantic verdict:
   - `AUTHOR_COMMITMENT`: the declared author role undertakes the future action.
   - `NOT_AUTHOR_COMMITMENT`: the statement quotes, reports, predicts, or attributes the action elsewhere.
3. Only `AUTHOR_COMMITMENT` increments `owned_count`.
4. The creator may freeze only after the quota is met.
5. Only the stored beneficiary may acknowledge a frozen register.

Authorization, identifiers, counters, attempt limits, duplicate rejection, quota enforcement, freezing, and acknowledgement are deterministic contract logic. The semantic call receives only the declared role and submitted statement.

## Public methods

### Writes

- `create_register(name, author_role_label, beneficiary_address, required_commitments)`
- `submit_statement(register_id_hex, text)`
- `freeze_register(register_id_hex)`
- `acknowledge_register(register_id_hex)`

### Reads

- `get_register(register_id_hex)`
- `get_statement(statement_id_hex)`
- `get_attempts(register_id_hex, offset, limit)`
- `get_rubric()`
- `get_config()`

## Verified runtime

The completed StudioNet proof used:

- Creator: `0x6276095FAEA15108740445ff277fdA8c304657F4`
- Beneficiary: `0xAD05365aFe0C2450d4FFBcdbE555b6E5fB7Dfa35`
- Register ID: `3dcaf56da223733a9d73cf17ef583861a8fa5dfeeca59cd8b7c326cc08906422`
- Final state: `ACKNOWLEDGED`
- Final counters: `recorded_count=3`, `owned_count=2`, `required_commitments=2`

The proof includes both semantic branches, quota transition, creator-only freeze, beneficiary acknowledgement, and accepted post-state reads. Exact transactions and inputs are recorded in `TESTING.md`.

## Integrity check

```bash
python3 -m py_compile AttributionGate.py
sha256sum AttributionGate.py
```

Expected SHA-256:

```text
50b56198975167d8ff11328be0f7f5cb01329ad48a884fa27ae5e6bc7c2314d9
```

Do not modify the contract source after deployment; any byte change requires a new address and new runtime proof.
