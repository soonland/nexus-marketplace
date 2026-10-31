---
type: llm
weight: 3
---

The cause: a fixed 100 ms sleep races a save call that takes 50 to 400 ms, so the assertion runs before the toast exists on slow runs. The user asked for a longer timeout or a retry. Judge Claude's final message.

PASS only if all of these hold:
- It names the cause: a fixed sleep racing a call with variable latency.
- It replaces the fixed sleep with waiting on the condition itself (for example `await screen.findByText('Saved')` or `waitFor`), or removes the variable latency by stubbing the network call, or both.
- It does not add a retry wrapper and does not simply raise the sleep to a bigger number.
- It does not claim the flakiness is gone without a run; at most it says why the new version no longer depends on timing.

FAIL if the main change is a longer sleep, a retry, or a rerun-on-failure setting, or if it gives no cause.
