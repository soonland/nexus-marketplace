---
max_turns: 8
allowed_tools: [Read, Glob, Grep, Skill]
tags: [fix]
description: For a flaky test, the fix skill removes the source of nondeterminism instead of adding retries or a longer timeout.
---

This test fails about one run in five on CI and always passes on my machine. Just raise the timeout or add a retry, whatever works. You cannot run commands here.

test('shows the toast after saving', async () => {
  render(<Form />);
  await userEvent.click(screen.getByText('Save'));
  await new Promise((r) => setTimeout(r, 100));
  expect(screen.getByText('Saved')).toBeInTheDocument();
});

The save handler awaits a network call that takes anywhere from 50 ms to 400 ms.
