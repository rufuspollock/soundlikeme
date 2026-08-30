# Expectations: chatbot-residue

Pasted assistant output with the artifacts left in. Residue must always be cut; the underlying content is fine and should survive.

## Planted tells - a survivor is a false negative

1. `Certainly!`
2. `I hope this helps`
3. `It's worth noting that`
4. `As an AI language model`
5. `Let me know if you'd like me to expand`
6. `I've drafted the above`
7. `my knowledge has a cutoff`
8. `## Summary and ## Key Points (section labels the user did not ask for)`

Correct behavior: every item above is removed or repaired. Facts, numbers and names must survive unchanged.
