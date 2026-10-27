# Assignment 01 — TA feedback: Samraj (Samraj Aneja)

- **Evaluated by:** Yilin Pan (TA)
- **Date:** 2026-10-05

## Scores

| Category | Pts | Student's LLM eval | TA verified | Notes |
|---|---|---|---|---|
| Immutability & encapsulation | 25 | 25 | 25 | |
| Constructor validation | 25 | 24 | 25 | |
| Correctness | 15 | 15 | 15 | |
| Testing | 15 | 11 | 12 | Loose asserts, unclear test names |
| Code quality & style | 10 | 6 | 8 | Duplicated switch in `toString()` |
| Introspection, LLM eval & code walk | 10 | 5 | 10 | `Species` is graded from the code walk |
| **Total** | **100** | **86** | **95** | |

## Checks

- [x] All tests pass (35/35)
- [x] No `UnsupportedOperationException` stubs left
- [x] Checkstyle warnings: 0
- [x] Coverage (lines / branches): 100% / 100%
- [x] Nothing out of scope (inheritance, collections, `equals`/`hashCode`)
- [ ] `submission/LLM-Evaluation.md` present (saved as `LLM-Evaluation-Template.md`)
- [ ] `submission/introspection.md` present (saved as `introspection-template.md`)

## Where the TA disagrees with the student's LLM evaluation

- Validation +1: the shared null/blank name message could be split in two, but it's not worth a point.
- Testing +1: I took −3 because the null-name message isn't tested, checks like `contains("age")`
  (AnimalTest.java:132) are too loose, and the names ending in "1" don't say what each test checks.
  Checking the messages was the right call. The problem is how they're asserted.
- Code quality +2: Checkstyle is clean and your lines are under 120. I took −2 for the duplicated
  `monthsString` switch (AgeMonths.java:133–145) and for setting it inside another switch.
- Cat 6 +5: explaining `Species` is part of the code walk, not the introspection, and your code walk covered it, so full credit.

## Feedback

The constructor and delegation are solid, and you tested `MAX_MONTHS - 1` and the max. In
`toString()`, one pluralize helper would replace the nested switches. You spotted this yourself. In
your introspection and LLM response, be more specific: name the actual invariants, and give a reason
for each point you accept or reject. Please also use the required filenames next time.

## Grade

**Final grade:** 95 / 100
