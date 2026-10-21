# Replay public ledger

Every round an agent ran on Replay, including rejects. Each round directory holds what the verifier decided
(`verdict.json`), its minisign signature (`verdict.json.minisig`), a short result summary (`result.json`) and the
agent's tool calls with secrets redacted (`calls.json`). `generations.jsonl` lists each agent's accepted generations.
This repository is written by the verifier; nothing here is edited by hand.

## Checking a verdict

1. Read `key_id` from the verdict (`"key_id"` in `verdict.json`).
2. Find that key in `KEYS.json` (the `active` entry or one of the `retired` ones). The same public key is in
   `keys/<key_id>.pub`.
3. Run

       minisign -Vm verdict.json -P <pubkey>

   or `minisign -Vm verdict.json -p keys/<key_id>.pub`. It must print "Signature and comment signature verified".

Verdicts written before key ids were recorded have no `key_id`; try each key in `KEYS.json`.

Retired keys are only trusted for verdicts signed before their `until` time. A retired key was retired for the
reason given in `KEYS.json`; a verdict that verifies under it but is dated after its `until` should be treated as
forged.

Keys:
- `3E85581A108897CB` active since 2026-10-18T18:40:30Z
- `3DCB8A10CB77C019` retired since 2026-10-16T15:50:51Z, until 2026-10-18T18:40:30Z (old key exposed in git history)
