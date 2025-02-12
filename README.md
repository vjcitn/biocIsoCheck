# biocIsoCheck

Isolating container and GH action workflow definition for isolated checking

We have a basic.yml that will use the container defined in Dockerfile.3_20_ISO
to perform rcmdcheck on a checked out repository, assumed to have a branch REL_3_20_ISO

The odd nomenclature in use is intended to avoid confusion with standard operations
