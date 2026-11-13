# clone-lin-maker-abacus-exp

Experimental **LIN** clone of MakerDAO / Sky `ExponentialDecrease.price` — Dutch-auction continuous per-second exponential decay (`price = rmul(top, rpow(cut, dur, RAY'))`).

**Proofs live in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open)** — this repo is the LIN copy only.

- **Upstream (hash-pinned, not vendored):** [makerdao/dss](https://github.com/makerdao/dss) `fa4f6630afb0624d04a003e920b0d71a00331d98`
  - `src/abaci.sol` sha256 `a4c3dc0368d66a5bf84e586f0cf76e2e80fb8b336c00348a97cc0d4ddcb016ca` blob `6cbd666267fc9f534ae52a45926eec5aa44b3bc5`
  - License AGPL-3.0-or-later. This repository does **not** vendor the Solidity source.
- **LIN kernel:** [`src/lin_maker_abacus_exp.lin`](src/lin_maker_abacus_exp.lin) (identical to lin-open)
- **C11 oracle copy:** [`test/oracles/maker_abacus_exp_c11.c`](test/oracles/maker_abacus_exp_c11.c)
- **Transpilable kernel:** [`test/oracles/maker_abacus_exp_kernel.c`](test/oracles/maker_abacus_exp_kernel.c)
- **Proofs (lin-open only):** `examples/maker_abacus_exp/` and `make maker-abacus-exp-proof`
- **Class:** EXPERIMENTAL uint64-scale. Not mainnet uint256 ray. Not a production abacus replacement.

Named goldens (`cut=0.99e9`): PAR `dur=0` → **1000000000**;
SEC1 `dur=1` → **990000000**;
SEC2 `dur=2` → **980100000**;
SEC3 `dur=3` → **970299000**;
NOT_HOLD `dur=89` → **408820172**;
ODD `top=123456789 dur=1` → **122222221**;
`cut>RAY'` fail-closes;
price is not wards-auth; LinearDecrease tau and stairstep `n=dur/step` are absent;
OVERFLOW `1e18*1e18` and `1e10*1e9` fail-close while naive i64 wraps.

## Reproduce proofs (in lin-open)

```
make maker-abacus-exp-proof
```

## Not claimed

Uint256 ray / mainnet ExponentialDecrease.price / Clipper.take parity.
LIN replacing Maker, Sky, clipper, or the EVM. TVL or dollar savings. zk soundness.
Hidden `cut` storage, `wards`/`file`, `block.timestamp`, LinearDecrease, or StairstepExponentialDecrease.
