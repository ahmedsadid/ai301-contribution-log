# Contribution 2: [docs] Improve website #1550 - Add timestamp to blog posts

**Contribution Number:** 2

**Student:** Zubayer Ahmed Sadid

**Issue:** https://github.com/facebook/stylex/issues/1550 

**Status:** Phase IV Complete
---

## Why I Chose This Issue

The issue had 3 parts, so I realized I could get 3 PRs out of it. It seemed relatively simple to implement, but with great impact. Plus. since they're visual changes on the documentation website, I was more intrigued as I hoped to see my changes live.

---

## Understanding the Issue

### Problem Description

Blog posts showed no publication date. The date already lived in each post's filename (YYYY-MM-DD prefix) and was used by the RSS feed, but was never shown to readers.

### Expected Behavior

Each post shows its date (e.g. June 23, 2026) under the title, matching the filename, with the RSS feed unchanged.

### Current Behavior

No date on the page. Date-parsing logic lived only inside rss.ts, so there was nothing to reuse and nothing stopping a page from parsing it differently.

### Affected Components

- src/lib/blog-date.ts — new helper (parseBlogDate, formatBlogDate)
- src/lib/rss.ts — refactored to use the helper
- Blog post page component — renders the date as a <time> element ⟪exact path since moved via Waku→fumadocs migration; confirm from PR's Files changed tab⟫
---

## Reproduction Process

### Environment Setup

My previous contribution was also in the same repo, so no new setup was needed. Just created a new branch in my local repo to work on the sub-issue.

### Steps to Reproduce

1. Created new branch of my fork.
2. Ran the docs app in packages/docs in local server.
3. Saw that blogposts had no timestamp.

### Reproduction Evidence

- **Commit showing reproduction:** n/a (commit squashed and merged following Meta's best practices standard)
- **Screenshots/logs:** <img width="1630" height="482" alt="2BA9ADBC-491C-4448-ACB9-2663E7814FDF_1_105_c" src="https://github.com/user-attachments/assets/7cd3dab6-4a82-4523-b6d3-2ec32a6fee58" />
- **My findings:** Timestamp was being used by the RSS feed, but the frontend of the docs site never included it.

---

## Solution Approach

### Analysis

Not a bug. The timestamp was never included in the site.

### Proposed Solution

Parse the blog post date from the filename, refactor `rss.ts` to import the correctly formatted date, then render `<time dateTime="…">` between title and authors.

### Implementation Plan

Using UMPIRE framework (adapted):

**Understand**: show the filename date on the page without breaking the feed or the timezone.

**Match**: reuse the RSS generator's existing date logic; follow the src/lib helper convention.

**Plan**: add helper → refactor rss.ts → render <time> → typecheck/lint/build + visual check.

**Implement**: https://github.com/facebook/stylex/pull/1734/changes/509ce1f328e678bf2b157572fa2dd7909e56f87d 

**Review**: CLA signed, feed unchanged, no duplicated logic, tsc/ESLint/Prettier clean.

**Evaluate**: serve the build, confirm date renders (light + dark), diff the RSS output.

---

## Testing Strategy

Manual testing only through visual checks.

### Manual Testing

Timestamps now showing. See before/after in PR description.

---

## Implementation Notes

Opened Jun 23, merged Jun 24.

### Code Changes

- **Files modified:** packages/docs/src/lib/blog-date.ts (new), src/lib/rss.ts (modified), blog post page component (modified)

---

## Pull Request

**PR Link:** https://github.com/facebook/stylex/pull/1734

**PR Description:** 

Blog posts didn't display a publication date. Each post now shows its date (e.g. June 14, 2026) under the title, parsed from the filename's YYYY-MM-DD prefix.

New src/lib/blog-date.ts (parseBlogDate / formatBlogDate) — a single source of truth for reading and formatting blog dates. Formatted in UTC to avoid an off-by-one day.
src/lib/rss.ts now uses the same helper instead of its own inline regex, so the page and the RSS feed can't drift apart (no behavior change — verified the feed's <pubDate> values are unchanged).
Rendered as a semantic <time dateTime="…"> element, spaced uniformly between the title and authors.


**Maintainer Feedback:**

None. Simply merged into production.

**Status:** Merged.

---

## Learnings & Reflections

### Technical Skills Gained

What RSS feeds are.

### Challenges Overcome

n/a

### What I'd Do Differently Next Time

n/a

---

## Resources Used

Claude Opus
