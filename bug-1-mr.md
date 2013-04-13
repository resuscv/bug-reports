
## Bug #1: mr

### Issue: The order that files are included matters

tl;dr - Changing the order that files are included in *.mrconfig* will 
mean that *lib = ...* defines will be lost.

* mr version: fa1f7770c9  (Log: releasing version 1.14)

- This bug report repository: https://github.com/resuscv/bug-reports
- Revisions:
  1. 980d95fe55
  2. 4c4703be9f

This repository contains
* .mrconfig
* mr-include/repo-[23].mr

where repo-2.mr has an additional *lib=...* defined.

##### First case
Looking at revision #1, the included files have repo-2.mr included 
first, which means that the *lib=some_text* is run for *mr up* commands:

```
mr update: /home/sandbox/repos/bbb
*****
*****  This worked
*****

mr update: /home/sandbox/repos/ggg

mr update: finished (2 ok)
```

In the second revision, we simply rename repo-3.mr to repo-1.mr so that 
it is imported first. The bug is triggered and the *lib = ...* is not 
included for the second and subsequent imports (it _is_ included for the 
first however, see *mr -v up* output).  Running *mr up*:

```
mr update: /home/sandbox/repos/bbb
sh: 45: some_text: not found

mr update: /home/sandbox/repos/ggg

mr update: finished (1 ok; 1 failed)
```

##### END
