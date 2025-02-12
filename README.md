# biocIsoCheck

Isolating container and GH action workflow definition for isolated checking.  A
key element is using prebuilt binaries from bioc2u and r2u, thus not working in
devel branch.

We have a basic.yml that will use the container defined in Dockerfile.3_20_ISO
to perform rcmdcheck on a checked out repository, assumed to have a branch REL_3_20_ISO

The odd nomenclature in use is intended to avoid confusion with standard operations

A basic question is how to balance the possible idiosyncrasies of developer contributions
with a concept of standard environment that builds and checks the vast majority of
packages.

As an example, Biostrings may need, in addition to the basic.yml at d5e466 of this repo.

```
      - name: additional latex packages
        run: (Rscript -e "tinytex::tlmgr_install('grfext')")
```

So far we have protected developers from concerns like this by providing extensive
coverage of all resources as needs emerge.

As I tried to use a simple action to work on DESeq2, I found it necessary to
add most of the components noted here:

```
RUN apt install -y libcurl4-openssl-dev
# for rcmdcheck via curl
RUN apt install -y zlib1g-dev
# for qpdf
RUN apt install -y libssl-dev
# for httr -> UCSC.utils -> GenomeInfoDb -> GRanges -> SE
RUN apt install -y libxml2-dev libpng-dev liblzma-dev libbz2-dev
#
```

Working on Biostrings suggests that various annotation packages aren't covered
in bioc2u, but it is not clear why the installation process doesn't get them.
