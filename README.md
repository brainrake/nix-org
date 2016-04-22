# Run Your Organization on NixOS

This series of articles will show you how to effectively run your company's
development process and infrastructure using Nix, NixOS, NixOps and Hydra.
It will save you time and headaches by eliminating vast minefields of failure
and largely or completely replacing the need for a myriad of technologies such as:

- package management - apt-get, yum, npm, pip, bundler, cabal ...
- "isolated" environments - nvm, rvm, virtualenv, nodeenv, stack ...
- package registries - pypi, rubygems, npm, bower,
- configuration management - puppet, chef, ansible, salt ...
- process management and monitoring - supervisord, monit, upstart, pm2 ...
- containers - docker, rkt
- deployment - puppet agent
- server provisioning - terraform
- OS - ubuntu, debian, coreos


### Introduction

Nix is a package manager and a pure functional language used to define,
configure and build packages. The inputs (dependencies) completely determine
the output (resulting package). This approach eliminates the single most
persistent and ubiquitous source of errors, failures, ambiguities, differences,
and undefined behaviour: state.

A system's current state, when not precisely defined, is a source of complexity
that cannot, even in principle, be accounted for. The result of evaluating a
function, or running a program, or building a package, depend on the complete
state of the system. Building the same package on a system with a different
configuration, although the dependencies are satisfied, can fail, or result in a
different output.

Package managers like apt-get and npm try to cope with this by asking maintainers to
list dependencies. But if a dependency is present but not listed, it will lead
to an irreproducible build, and a package that doesn't work.

Configuration management systems like puppet and chef try to cope by
gathering information about the current configuration and trying to determine
the difference compared to the requested configuration, but this fails because
the state is dependent on all  of the previous states, and the described
configuration doesn't account for the influence of all that is _not_ specified.

Nix sidesteps these problems by following through with being purely functional.
Only the inputs determine the outputs. When run with the same inputs, the output
is always the same.

System state is irrelevant. Discovering the status quo is unnecessary.
If something is not specified, it does not exist, so it cannot influence reality.
There are no unknowns, so we don't have to think about them.
Such is the power of purity.

This simple principle, applied to package management, leads to reproducible builds
that once built, will not break, regardless of what else is going on in the system.
Packages can only see specified inputs (dependencies), so nothing else can affect
the build output, wether it succeeds, or what is available at runtime.

Different versions of libraries can coexist witout any problems.
Different environments can live side by side without any interference.
Development environments can finally be truly isolated, not only in name and intent.
Installs, updates, and all changes in general are atomic. They either succeed,
or they don't, in which case no changes at all are applied.

NixOS applies this principle to the whole operating system, using the same
Nix language to build a reproducible, immutable, fully specified OS.
It manages system configuration, services, users, and much more, and given a
configuration, always results in the same system, no matter where you start from.
Not because it finds the difference and patches it, but because conceptually,
it always starts from scratch, with no assumptions and no influence of the past.

NixOps in turn is a tool for deploying NixOS configurations on cloud servers.
It replaces cloud provisioning and deployment tools and brings the same advantages
of statelessness, purity, immutability and reproducibility to a cloud workflow,
so you can run anything at scale.

Hydra ties all this together by building nix packages and providing a binary
cache, so you can download packages instead of build them for every install,

Together, these tools allow you to run your whole software infrastructure:

- develop in isolated, reproducible nix environments
- automatically build, test, and publish packages with hydra and nix channels
- fully and declaratively specify immutable, atomically upgradable servers with NixOS
- provision and deploy servers and services on the cloud with NixOS


### Package Management and Configuration with Nix

- Status: **complete**
- Difficulty: **easy**
- Replaces:
  - general package management - apt-get
  - mostly: language-specific package management: npm, pip, bundler, cabal
  - build tools: make, rake, setuptools, npm,


### Develop, Build and Test Your Software with Nix

- Status: **complete**
- Difficulty: **easy**
- Replaces:
  - general package management
  - language-specific package management (mostly): npm, pip, bundler, cabal
  - third-party package registries and repositories: npm, pypi,
  - build tools: make, rake, setuptools, npm,


### Declare Your System with NixOS

- Status: **complete**
- Difficulty: **easy**
- Replaces:
  - OS - debuntu, redhat, coreos, arch (yes, you too)
  - package management
  - process management


### Run Your Software on NixOS with SystemD

- Staus: **complete**
- Difficulty: **easy**
- Replaces:
  - package management
  - process management
  - language-specific "isolated" environments
  - third party registries and repositories


### Provision Cloud Servers with NixOps

- Status: **fairly complete** but not many clouds are supported
- Difficulty: **easy**
- Replaces:
  - configuration management: puppet, chef, ansible
  - deployment management: puppet agent, ansible
  - provisioning: terraform, ...


### Automate Builds and Serve Packages with Hydra and Nix Channel

- Status: **needs integration**
- Difficulty: **medium** needs manual setup and build :(
- Replaces:
  - continuous integration/deployment - jenkins, travis, strider
  - package repositories - dpkg, npm, pypi, rubygems, hackage
- Advantage:
  - reproducible builds
  - cryptographically signed binary caches of source packages



### All Together

