
# Mini Proof-of-Work Blockchain — Python

An educational, batteries-included mini blockchain to showcase the **UTXO** model, **ECDSA signatures**, a simple **mempool**, **coinbase rewards** with halvings, and **difficulty adjustment** targeting ~30s blocks.

> ⚠️ This is for learning. Do **not** use in production or with real value.

## Features
- Blocks: header (prev_hash, merkle_root, timestamp, target, nonce) + tx list
- PoW: `double_sha256(header)` ≤ `target` (adjusts every 10 blocks toward 30s target)
- UTXO model: inputs reference previous outputs; signatures over tx preimage
- Coinbase: block subsidy with halving every 100 blocks
- Mempool: basic in-memory queue persisted to `data/`
- CLI wallet: keygen, balance, send, mine, status
- Minimal persistence: chain, utxo, mempool saved in `./data`

## Quick start
```bash
python -m venv .venv && source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -r requirements.txt

# Inspect / init (genesis auto-created)
python -m minipow.cli status

# Generate a wallet
python -m minipow.cli keygen > mykey.txt
PRIV=$(grep priv_hex mykey.txt | cut -d: -f2 | tr -d ' ')
ADDR=$(grep address mykey.txt | cut -d: -f2 | tr -d ' ')

# Mine a block to yourself
python -m minipow.cli mine --miner-address $ADDR
python -m minipow.cli balance --address $ADDR

# Send value and mine it
python -m minipow.cli send --from-priv $PRIV --to SOMEOTHERADDRESS --amount 5 --hint-address $ADDR
python -m minipow.cli mine --miner-address $ADDR
```

## Design parameters
- Target block time: **30 seconds**
- Difficulty adjust interval: **10 blocks**
- Halving interval: **100 blocks**
- Initial subsidy: **50** units
- Address: Base58Check of `ripemd160(sha256(pubkey))`

## Test
```bash
python -m pytest -q
```

## License
MIT
