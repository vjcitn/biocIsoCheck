# biocIsoCheck

Isolating container and GH action workflow definition for isolated checking.  A
key element is using prebuilt binaries from bioc2u-builder (thus also from r2u), thus **not working in
devel branch**.

We have a basic.yml that will use the container defined in D4bioc2uJammy
to perform rcmdcheck on a checked out repository, assumed to have a branch REL_3_20_ISO

The odd nomenclature in use is intended to avoid confusion with standard operations

A basic question is how to balance the possible idiosyncrasies of developer contributions
with a concept of standard environment that builds and checks the vast majority of
packages.

Example 1: the trivial parody package was [checked](https://github.com/vjcitn/parody/actions/runs/13326835361) in 1m42s

Example 2: DESeq2 [checked](https://github.com/vjcitn/DESeq2/actions/runs/13329655502/job/37230556460) at commit 630250 of this repo, in 8m25s

Example 3: minfi [checked](https://github.com/vjcitn/minfi/actions/runs/13330007240) at commit 630250 of this repo, in 12m58s

Example 4: Biostrings [check](https://github.com/vjcitn/Biostrings/actions/runs/13329964997) in 9m51s

Example 5: GenomicRanges ran into 
```
! LaTeX Error: File `beramono.sty' not found.
Type X to quit or <RETURN> to proceed,
or enter new name. (Default extension: sty)
! Emergency stop.
<read *> 
         
l.94 \RequirePackage
                    [T1]{fontenc}^^M
!  ==> Fatal error occurred, no output PDF file produced!
--- failed re-building ‘GenomicRangesHOWTOs.Rnw’
--- re-building ‘GRanges_and_GRangesList_slides.Rnw’ using Sweave
Warning in .merge_two_Seqinfo_objects(x, y) :
  The 2 combined objects have no sequence levels in common. (Use
  suppressWarnings() to suppress this warning.)
Error: processing vignette 'GRanges_and_GRangesList_slides.Rnw' failed with diagnostics:
Running 'texi2dvi' on 'GRanges_and_GRangesList_slides.tex' failed.
LaTeX errors:
! LaTeX Error: File `beamer.cls' not found.
```

Here we have to decide whether to enrich the building container or to leave this to the developer.

Example 6: limma passed but gave
```
❯ checking Rd cross-references ... NOTE
  Package unavailable to check Rd xrefs: ‘edgeR’
```

Example 7: Rgraphviz, passed but
```
❯ checking compiled code ... WARNING
  File ‘Rgraphviz/libs/Rgraphviz.so’:
    Found ‘__sprintf_chk’, possibly from ‘sprintf’ (C)
      Objects: ‘LL_funcs.o’, ‘agopen.o’, ‘buildEdgeList.o’
  
  Compiled code should not call entry points which might terminate R nor
  write to stdout/stderr instead of to the console, nor use Fortran I/O
  nor system RNGs nor [v]sprintf.
  
  See ‘Writing portable packages’ in the ‘Writing R Extensions’ manual.

❯ checking if this is a source package ... NOTE
  Found the following apparent object files/libraries:
    src/graphviz/libltdl/lt__strl.lo src/graphviz/libltdl/lt__strl.o
    src/libwin/i386/lib/libcdt.a src/libwin/i386/lib/libcdt.la
    src/libwin/i386/lib/libgraph.a src/libwin/i386/lib/libgraph.la
    src/libwin/i386/lib/libgvc.a src/libwin/i386/lib/libgvc.la
```

The associated Dockerfile is [here](https://github.com/vjcitn/biocIsoCheck/blob/main/D4bioc2uJammy)
and the current action workflow is [here](https://github.com/vjcitn/biocIsoCheck/blob/main/basic.yml).


