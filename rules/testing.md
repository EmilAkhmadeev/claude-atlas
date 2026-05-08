---
alwaysApply: true
---

# Testing

- Tests must verify behavior, not implementation details.
- Run related test file after changes, not full suite.
- Flaky test: fix or delete it, never retry to make it pass.
- Prefer real implementations over mocks. Only mock at system boundaries (network, filesystem, clock).
- One assertion per test. If name needs "and" -- split it.
- Test names describe behavior. No `test1` or similar.
- Use Arrange-Act-Assert structure. No logic (if/loops) in tests.
- Never `expect(true)` or assert a mock was called without checking its arguments.
