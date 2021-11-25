# How to build LLVM inside a Windows container

## Problem

Creating a Dockerfile that `ADD`s the source code inside is problematic, it works well only for Process Isolation machines and not Hyper-V ones.
`docker build` on Windows with Hyper-V uses only 2 cores and that's the situation for almost everyone using Docker, LLVM would take ages to compile.

## Solution

1. Build Docker image that is ready to build LLVM
2. Run it only to build LLVM by mounting inside the source code (`docker run` can use all cores when `NUMBER_OF_PROCESSORS` is specified)
3. Get the artifacts out using a volume

## F.A.Q.

### `CVTRES : fatal error CVT1107: something.lib is corrupt` or `LINK : fatal error LNK1123: failure during conversion to COFF: file invalid or corrupt`

If you get similar errors or related to PDB or deleting files this could be a problem of using volumes with Hyper-V, build LLVM entirely in a directory inside the container and only at the end copy the artifacts to a folder mounted outside