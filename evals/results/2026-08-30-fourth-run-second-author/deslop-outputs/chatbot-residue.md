The migration moved 14 services from the legacy scheduler to the new one over six weeks. Three services required schema changes; the rest needed only a config update.

The rollback plan was never exercised, which is a good sign but also means it remains untested.

The one real problem was the billing service, which held open database connections across the cutover and had to be restarted twice. The exact timings here come from the deploy log.
