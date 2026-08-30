# Expectations: quoted-slop

Someone else's marketing slop, quoted. Everything inside the blockquote and inside quotation marks is evidence: editing it falsifies a quotation. The reporter's own prose is plain and has no defect to repair.

## Protected spans - an edit to any of these is a false positive

1. `In today's fast-paced digital landscape, Halcyon Grid stands as a testament`
2. `delve into the intricate tapestry of enterprise workflow`
3. `It's not just a platform, it's a paradigm shift. Let that sink in.`
4. `pricing is "available on request."`
5. `the system remains robust well past that threshold`
6. `working to unlock the full potential of our incident response process going forward`
7. `$60m`
8. `about 90 people`

Correct behavior: a no-op, or an edit so small it touches none of the spans above. `deslop` should report no confirmed findings.
