# Contribution 1: @stylexjs/valid-styles: gridColumn / gridRow must be a stringified number

**Contribution Number:** 1  
**Student:** Zubayer Sadid  
**Issue:** https://github.com/facebook/stylex/issues/1701  
**Status:** Phase IV Complete


---

## Why I Chose This Issue

This issue seemed simple enough to work on as a first time contributor, but at the same time it looks like a solving it would have a meaningful impact to the repo. I have some understanding of TypeScript already, so it shouldn't be too difficult for a first contribution, plus in my experience, AI is pretty reliable for web frameworks. It's also a very new issue, so I hope I have better odds at getting feedback and potentially a merge!

---

## Understanding the Issue

### Problem Description

[In your own words, what's broken or missing?]

### Expected Behavior

[What should happen?]

### Current Behavior

[What actually happens?]

### Affected Components

[Which parts of the codebase are involved?]

---

## Reproduction Process

### Environment Setup

Environment setup was pretty straightforward by following the contributing guide on the repo. However, the guide had stale documentation and did not mention the correct minimum Node version. I opened an issue for it and updated the version on the contributing guide.

I just needed to install yarn and node.

### Steps to Reproduce

1. Navigate to the ESLint plugin package:
cd packages/@stylexjs/eslint-plugin
2. Open the test file __tests__/stylex-valid-styles-test.js and add the following entry to the top of the first valid: [ array. This asserts that a numeric grid value should be accepted:
`
  import * as stylex from '@stylexjs/stylex';
  const styles = stylex.create({
    foo: {
      gridColumnStart: 1,
    }
  });
`,
3. Run the linter's test suite:
npx jest stylex-valid-styles

Expected result: The test passes — gridColumnStart: 1 is valid CSS, so the linter should accept it.

Actual result: The test fails. The linter reports an error on the numeric value:
gridColumnStart value must be one of:
auto
a string literal
null
initial
inherit
unset
revert
Tests: 1 failed, 170 passed, 171 total

### Reproduction Evidence

- **Commit showing reproduction:** https://github.com/ahmedsadid/stylex/commit/64e0bce9
- **Screenshots/logs:** [If applicable]
  <img width="902" height="723" alt="image" src="https://github.com/user-attachments/assets/92a1271f-e763-4102-8782-4206ba54f0fc" />

- **My findings:** Nothing in particular other than already stated info.


---

## Solution Approach

### Analysis

[Your analysis of the root cause - what's causing the issue?]

### Proposed Solution

[High-level description of your fix approach]

### Implementation Plan

The grid rule in cssProperties.js (line 332) only allows auto and text — it forgot to allow numbers. I'll add isNumber to that one rule, which fixes all six grid properties at once (since they all share it). Then I'll add a couple of tests with numbers like gridColumnStart: 1 to prove it works, and run npx jest stylex-valid-styles to confirm everything passes.

**Understand:** The ESLint rule `stylex-valid-styles` rejects numbers like
`gridColumnStart: 1`, even though they're valid CSS. It affects all six grid-line
properties (gridColumn/Row + their Start/End variants).

**Match:** The same file already does this for `zIndex` (allows `auto` or a number),
and PRs #1522/#1523 added bare-number support to other properties. The bug: the shared
`gridLine` rule allows `auto` and strings but forgot numbers.

**Plan:**
1. Add `isNumber` to the `gridLine` rule in `cssProperties.js` (fixes all six properties).
2. No new function needed — `isNumber` already exists.
3. Add tests for numeric grid values, including a negative one (`gridRowStart: -1`).

**Implement:**
- Branch: https://github.com/ahmedsadid/stylex/tree/issue-1701
- Commit: [`fc844ab1`](https://github.com/ahmedsadid/stylex/commit/fc844ab1)
- PR: [‹PR link›](https://github.com/facebook/stylex/pull/1730)

**Review:**
- [x] Atomic, targeted changes following existing conventions
- [x] Includes tests

**Evaluate:** Wrote a failing test first, then confirmed the fix passed. Full suite
passes (548 tests), plus flow, prettier, and lint — matching CI.


---

## Testing Strategy

- Reproduced the bug with a failing test, then confirmed the fix made it pass.
- Used the plugin's `RuleTester` suite (`npx jest stylex-valid-styles`).
- Covered all six properties, including a negative value (`gridRowStart: -1`).
- Ran the full suite (548 passing) plus flow, prettier, and lint before pushing.

---

## Implementation Notes

### Week 3 Progress

- Found that the `stylex-valid-styles` ESLint rule rejected numbers like
  `gridColumnStart: 1`, even though they're valid CSS.
- Traced it to one shared rule, `gridLine`, in `cssProperties.js`, which allowed
  only `auto` and strings — not numbers.
- Wrote a failing test first to confirm the bug.
- Made changes to the code to fix the bug.
- Fixed it by adding `isNumber` to `gridLine`, which fixes all six grid-line
  properties at once.
- Added tests, rebased onto upstream, squashed to one commit, and opened a PR.

### Code Changes

- **Files modified:**
  - `packages/@stylexjs/eslint-plugin/src/reference/cssProperties.js`
  - `packages/@stylexjs/eslint-plugin/__tests__/stylex-valid-styles-test.js`
- **Key commit:** [`fc844ab1`](https://github.com/ahmedsadid/stylex/commit/fc844ab1) — Fix #1701
- **Approach decisions:** Fixed the shared `gridLine` rule instead of each property; used
  `isNumber` to match the existing `zIndex` convention.

---

## Pull Request

**PR Link:** https://github.com/facebook/stylex/pull/1730

**PR Description:**  The `stylex-valid-styles` rule rejected numeric literals for grid line properties, even though they are valid CSS. This happened because the shared `gridLine` rule in `cssProperties.js` only permitted `auto` and string values, not numbers. This change adds `isNumber` to the `gridLine` rule. Because all six grid line properties — `gridColumn`, `gridColumnStart`, `gridColumnEnd`, `gridRow`, `gridRowStart`, `gridRowEnd` (and `gridArea`) — derive from `gridLine`, the single change fixes them all. 

**Maintainer Feedback:**

None so far.

- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** Awaiting Review

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
