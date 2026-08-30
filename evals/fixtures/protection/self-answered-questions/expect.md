# Expectations: self-answered-questions

The self-answered rhetorical question is listed in tells.md as a HARD tell, which fires on sight. It is also the principal structural device of the author of this passage, at roughly one question every two hundred words, and the answers are substantive rather than dramatic. Protecting it is the point of the fixture.

## Protected spans - an edit to any of these is a false positive

1. `Why? I think the answer is that trying hard`
2. `Does that mean effort does not matter? Obviously not.`
3. `What happens to that? School, mostly.`
4. `Can you get rid of it? Some people manage.`
5. `That seems worth explaining.`
6. `The difference sounds like hair-splitting and is not.`
7. `about ten years too early`

Correct behavior: a no-op, or an edit so small it touches none of the spans above. `deslop` should report no confirmed findings.
