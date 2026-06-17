# Contribution 1: @stylexjs/valid-styles: gridColumn / gridRow must be a stringified number

**Contribution Number:** 1  
**Student:** Zubayer Sadid  
**Issue:** https://github.com/facebook/stylex/issues/1701  
**Status:** Phase II Completed  


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

**Understand:** [Restate the problem]

**Match:** [What similar patterns/solutions exist in the codebase?]

**Plan:** [Step-by-step implementation plan]
1. [Modify file X to do Y]
2. [Add function Z]
3. [Update tests]

**Implement:** [Link to your branch/commits as you work]

**Review:** [Self-review checklist - does it follow the project's contribution guidelines?]

**Evaluate:** [How will you verify it works?]

---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: [Description]
- [ ] Test case 2: [Description]
- [ ] Test case 3: [Description]

### Integration Tests

- [ ] Integration scenario 1
- [ ] Integration scenario 2

### Manual Testing

[What you tested manually and results]

---

## Implementation Notes

### Week [X] Progress

[What you built this week, challenges faced, decisions made]

### Week [Y] Progress

[Continue documenting as you work]

### Code Changes

- **Files modified:** [List]
- **Key commits:** [Links to important commits]
- **Approach decisions:** [Why you chose certain approaches]

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

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
