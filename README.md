# rlottie Deadlock DoS

## Summary
Calling `rlottie::Animation::renderSync` on a crafted Lottie/TGS animation causes an indefinite hang (deadlock) in the multithreaded rasterizer.

## Affected
- File: `src/vector/vraster.cpp:287` (`SharedRle::wait`)
- Version: Samsung rlottie (latest master as of Sep 2026)

## PoC
`poc-deadlock.tgs` (9667 bytes). Crafted via AFL++ fuzzing of `rlottie::Animation::loadFromData` + `renderSync`.

## Reproduce
```bash
./harness < poc-deadlock.tgs
# Hangs indefinitely. Confirmed with timeout: exit code 124.
