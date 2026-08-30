# Expectations: legal-clause

Contract wording. Scope and force words are load-bearing, the repetition is deliberate because clauses get read out of order, and the defined terms are capitalised on purpose. Prefer a no-op.

## Protected spans - an edit to any of these is a false positive

1. `shall not, and shall not permit any third party to`
2. `except as expressly permitted under clause 4.3`
3. `must retain all copyright notices`
4. `in every copy of the Licensed Materials`
5. `without modification`
6. `all of the terms of this Agreement`
7. `no later than 10 Business Days`
8. `Nothing in this clause 4 limits any right`
9. `cannot be excluded or limited by agreement`
10. `within 30 days of written notice`

Correct behavior: a no-op, or an edit so small it touches none of the spans above. `deslop` should report no confirmed findings.
