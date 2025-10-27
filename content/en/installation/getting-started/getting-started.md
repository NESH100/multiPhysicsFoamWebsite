---
title: 'Getting Started'
date: 2025-09-27
type: 'docs' # <- keeps blog layout
disableToc: true
description: 'Step-by-step guide for beginners'
authors: ['Naresh']
# tags: ['installation', 'setup']
cascade:
  disableToc: true
---

# multiRegionFoam Installation Guide

{{% pageinfo %}} This guide provides detailed instructions for installing
multiRegionFoam, a modular framework for multiphysics simulations in OpenFOAM.
{{% /pageinfo %}}

## Prerequisites

{{% alert title="Important Requirement" color="warning" %}} **multiRegionFoam
currently only compiles with foam-extend-4.1**

Portation to other OpenFOAM forks is currently undergoing development.
{{% /alert %}}

## Step 1: Install foam-extend-4.1

### Download and Install foam-extend-4.1

Follow the official installation instructions available at:

```
https://openfoamwiki.net/index.php/Installation/Linux/foam-extend-4.1
```

### Fix for Newer Compilers

{{% alert title="Compiler Compatibility Fix" color="info" %}} To compile
foam-extend-4.1 with newer gcc and g++ compilers, you need to make a small
modification to the source code. {{% /alert %}}

**File to modify:**

```bash
src/foam/containers/Lists/PackedList/PackedList.H
```

**Required change:**

```cpp
class PackedList
:
public PackedListCore,
private public List<unsigned int>  // Change 'private' to 'public'
```

## Step 2: Download multiRegionFoam

### Set Up Environment

```bash
# Activate foam-extend-4.1 environment
$ fe41

# Create the run directory if it doesn't exist
$ mkdir -p $FOAM_RUN

# Navigate to parent directory of run folder
$ cd $FOAM_RUN/..
```

### Clone Repository

```bash
# Clone the multiRegionFoam repository
$ git clone https://bitbucket.org/hmarschall/multiregionfoam.git

# Change to the multiRegionFoam directory
$ cd multiRegionFoam
```

## Step 3: Compile multiRegionFoam

### Build the Framework

```bash
# Run the compilation script
$ ./Allwmake
```

{{% alert title="Compilation Notes" color="info" %}} The compilation process
will:

- Build all necessary libraries
- Compile the main multiRegionFoam solver
- Set up the framework components
- Display installation instructions upon completion {{% /alert %}}

### Follow Terminal Instructions

After running `./Allwmake`, carefully follow any additional instructions
displayed in the terminal to complete the installation.

## Step 4: Verify Installation

### Test the Installation

```bash
# Check if multiRegionFoam is properly installed
$ multiRegionFoam -help
```

### Expected Output

You should see something similar to:

```
Usage: multiRegionFoam [OPTIONS]
options:
-DebugSwitches <key1=val1,key2=val2,...>
-DimensionedConstants <key1=val1,key2=val2,...>
-InfoSwitches <key1=val1,key2=val2,...>
-OptimisationSwitches <key1=val1,key2=val2,...>
-parallel
-roots <(dir1 .. dirN)>
-case <dir>
-help
-doc
-srcDoc
```

{{% alert title="Installation Success" color="success" %}} If you see the usage
information displayed above, multiRegionFoam has been successfully installed and
is ready to use! {{% /alert %}}

## Troubleshooting

### Common Issues

#### Issue 1: foam-extend-4.1 Not Found

**Error:** `fe41: command not found`

**Solution:**

- Ensure foam-extend-4.1 is properly installed
- Source the foam-extend environment:
  ```bash
  source /path/to/foam-extend-4.1/etc/bashrc
  ```

#### Issue 2: Compilation Errors with Newer Compilers

**Error:** Compilation fails with newer GCC versions

**Solution:**

- Apply the PackedList.H fix mentioned in Step 1
- Ensure you've changed `private` to `public` in the class declaration

#### Issue 3: Git Clone Fails

**Error:**
`fatal: repository 'https://bitbucket.org/hmarschall/multiregionfoam.git' not found`

**Solution:**

- Check your internet connection
- Verify the repository URL is correct
- Try using SSH instead:
  `git clone git@bitbucket.org:hmarschall/multiregionfoam.git`

#### Issue 4: Compilation Warnings

**Warning:** Various compiler warnings during build

**Solution:**

- Warnings are typically non-critical
- Ensure the build completes successfully
- Test with `multiRegionFoam -help` to verify installation

### Getting Help

If you encounter issues not covered here:

1. **Check the Repository Issues**: Visit the Bitbucket repository issues
   section
2. **Community Support**: Reach out to the OpenFOAM community forums
3. **Documentation**: Review the complete multiRegionFoam documentation

## Next Steps

Once installation is complete, you can:

1. **Run Tutorial Cases**: Explore the provided tutorial cases in the
   `tutorials/` directory
2. **Read Documentation**: Study the framework overview and theory
3. **Start Your First Simulation**: Begin with simple conjugate heat transfer
   cases

{{% alert title="Ready to Start" color="success" %}} With multiRegionFoam
successfully installed, you're ready to tackle complex multiphysics simulations
with interface coupling! {{% /alert %}}
