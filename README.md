# Run Your Organization on Nix

This series of articles will show you how to effectively run your company's
development process and infrastructure using Nix and friends.
It will save you time and headaches by eliminating vast minefields of failure
and largely or completely replacing the need for a variety of technologies such as:

- package management - apt-get, yum, maven, npm, pip, bundler, cabal ...
- "isolated" environments - nvm, rvm, virtualenv, nodeenv, stack ...
- package registries - pypi, rubygems, npm, bower, hackage
- configuration management - puppet, chef, ansible, salt ...
- deployment - puppet master, ansible ...
- process management and monitoring - supervisord, monit, upstart, pm2 ...
- containers - docker, rkt
- server provisioning - fog, terraform, cloudformation, heat, boto ...
- operating system - ubuntu, debian, coreos ...

Many of these tools try to solve closely related problems in slightly
different contexts. They all work reasonably well upto a point, but then the
complexity becomes untraceable. They all fail in the same way: most operations
rely on the current state of the system, which in turn is the result of previous
operations, as well as outside effects that the tool in question does not know
about.

Nix uses a fundamentally different approach. It is purely functional, meaning
that in any context, the inputs (and nothing else) completely and unambiguously
determine the output. Nix takes this simple principle to its conclusion by
applying it pervasively, with great success.

System state is irrelevant. Discovering the status quo is unnecessary.
If something is not specified, it does not exist, so it cannot influence reality.
There are no unknowns, so we don't have to think about them.
Such is the power of purity.

The following articles describe how Nix applies to many aspects of your
organization's technology process: development, build, test, deployment,
configuration, provisioning - essentially, all of DevOps. For each use, they will
show what tools are replaced or tamed, what problems purity eliminates, what
opportunities it creates, and how exactly to use Nix and friends in that role,
using a running example - a SaaS developed with

Of course, Nix is not a panacea. There are problems it cannot solve, and it has
a few problems of its own. The large number of problems that it does solve, and
the flexibility and safety gained make it highly worthwile.


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

[Nix](https://nixos.org/nix/) sidesteps these problems by following by being purely functional.
Only the inputs determine the outputs. When run with the same inputs, the output
is always the same.

Applied to package management, this principle leads to reproducible builds
that once successfully built, cannot break, regardless of what else is going on
in the system. Packages can only see specified inputs (dependencies), so nothing
else can affect the build output, wether it succeeds, or what is available at runtime.
Packages are also immutable. They cannot change

Different versions of libraries can coexist witout any problems.
Different environments can live side by side without any interference.
Development environments can finally be truly isolated, not only in name and intent.
Installs, updates, and all changes in general are atomic. They either succeed,
or they don't, in which case no changes at all are applied.

[NixOS](https://nixos.org/) applies this principle to the whole operating system,
using the same Nix language to build a reproducible, immutable, fully specified OS.
It manages system configuration, services, users, and much more, and given a
configuration, always results in the same system, no matter where you start from.
Not because it finds the differences and patches them, but because conceptually,
it always starts from scratch, with no assumptions and no influence of the past.

[NixOps](https://nixos.org/nixops/) in turn is a tool for deploying NixOS configurations on cloud servers.
It replaces cloud provisioning and deployment tools and brings the same advantages
of statelessness, purity, immutability and reproducibility to a cloud workflow,
so you can run anything at scale.

[Hydra](https://nixos.org/hydra/) ties all this together by building nix packages and providing a binary
cache, so you can download packages instead of building them for every install,

Together, these tools allow you to run your whole software infrastructure:

- develop in isolated, reproducible nix environments
- automatically build, test, and publish packages with hydra and nix channels
- fully and declaratively specify immutable, atomically upgradable servers with NixOS
- provision and deploy servers and services on the cloud with NixOps


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
  - deployment management: puppet master, chef, ansible
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

