---
max_turns: 8
allowed_tools: [Read, Glob, Grep, Skill]
tags: [fix]
description: Given a vague report with no steps and no expected result, the fix skill asks for the missing details instead of guessing a fix.
---

Fix this: sometimes customers on the checkout page get charged the wrong amount. Support noticed it a few weeks ago. I don't have more details than that. You cannot run commands here. The checkout total is computed here:

export function checkoutTotal(items: { price: number; qty: number }[], couponPct = 0): number {
  const subtotal = items.reduce((sum, i) => sum + i.price * i.qty, 0);
  return subtotal * (1 - couponPct / 100);
}
