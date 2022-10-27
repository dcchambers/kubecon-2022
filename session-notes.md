# Session Notes and Thoughts

## Contents

- [Learn About Helm and its Ecosystem](#learn-about-helm-and-its-ecosystem)
- [Understanding the Feature Lifecycle In Kubernetes](#understanding-the-feature-lifecycle-in-kubernetes)

---

## Learn About Helm and its Ecosystem

- **Title**: Learn About Helm and its Ecosystem 
- **Presenters**: Andrew Block & Karena Angell, Red Hat; Matt Farina, SUSE; Scott Rigby, Weaveworks

### Helm: Package manager for Kubernetes.

More to the concept than just "packaging something up." It allows someone that isn't intimitely familiar with underlyding systems to install/run/be sucessful with a tool.

Example: Wordpress
- If you were to install and run WP yourself via K8s, it requires knowledge of: Wordpress itself, secrets, statefulSet, Service, HPA, Deployment, and Network Policies.
- You can create a 'package' that manages/orchestrates all of these things.

### Helm: 3 Parts

1. Charts - the packages themselves.
2. Helm Client (CLI)
3. Helm SDK

### Helm is Stable Software

Backwards-compatible guarantee.

Helm follows semantic versioning.
- Minor releases 3 times per year.

Helm is a CNCF graduated project.

### The Ecosystem

Examining the ecosystem stack:

```mermaid
flowchart TD
    A --- B --- C
    B --- D
    C --- E
    D --- E
    A[Configuration Management]
    B[Package Management]
    C[Binaries]
    D[Config]
    E[Operating System]
```

A typical linux ecosystem looks something like this:

```mermaid
flowchart TD
    A --- B --- C
    B --- D
    C --- E
    D --- E
    A[Chef, Puppet, Ansible, etc]
    B[apt, zypper, yum, etc]
    C[ELF Binaries]
    D["/etc"]
    E[GNU/Linux]
```

A typical kubernetes example looks like this:

```mermaid
flowchart TD
    A --- B --- C
    B --- D
    C --- E
    D --- E
    A[A bunch of options...]
    B[Helm]
    C[Images]
    D[K8s Resources]
    E[Kubernetes]
```

#### Platform/app management (top level) examples:
- Flux
- Argo
- Helmfile
- Terraform

### Chart Storage
- Push charts to container registries (Any OCI registries that supports artifacts)
- Harbor
- Helm has a plugin system so you can store charts in other places (eg S3)

### Discovery
How are you finding your charts?
- Helm Hub has been deprecated
- Artifact Hub created to replace Helm Hub.
  - Supports Helm Charts
  - Operators
  - Plugins
  - and much more!
- Many other places...
  - Bitnami
  - Nvidia Catalog
  - Rancher Charts Catalog
  - Red Hat Openshift Dev Console
  - etc...

### Developer Tools

How to integrate Helm into your development workflows?

- Helm SDK (golang)
- Generate JSON schema from values
  - plugins to do this...
- Helm react UI
- Red Hat Chart Verifier
- Chart Testing
- CI: Helm Project Actions
  - And many more in Github Actions catalogue
- IDE Integration
  - VS Code Extension
  - Vim Plugins
    - vim-kubernetes
    - vim-helm

### Getting Involved

- Build projects on top of Helm.
- Look at Helm Plugins
- Helm Source Code
  - Always looking for people to fix bugs, implement features, contribute
- [Helm Website](https://helm.sh)
- Channels in Kubernetes Slack
  - #helm-users
  - #helm-dev
  - #charts
- Mailing List: https://lists.cncf.io/g/cncf-helm
- Community Meeting on Thursday

---


## Understanding the Feature Lifecycle In Kubernetes

- **Title**: “Why Can’t Kubernetes Devs Just Add This New Feature? Seems So Easy!” - Understanding the Feature Lifecycle In Kubernetes
- **Presenters**: Ricardo Katz, VMware & Carlos Panato, Chainguard

It's not unusual for it to take *years* for feature requests to make it into a K8s release.

- Why does it take so long?
- Why are some feature requests refused?

Questions to be answered for all feature requests:
- Does this solve a wide problem or is it a niche/specific use case?
- Does the feature bring (or fix) security issues?
- Does the feature bring performance improvements or concerns?
- Is the feature a breaking change? (Not allowed in GA!)
- Has the feature been discuseed before? What was the outcome of that discussion?

### KEP (Kubernetes Enhancement Proposal)

A way to propose, communicate, and coordinate on new efforts for the Kubernetes project itself.

This process is still in beta but is mandatory for all proposals starting in K8s 1.14.

### The Process

```mermaid
flowchart TD
    A --> B --> C --> C --> D --> E --> E --> F --> G --> G --> H
    A["Discuss with SIG (Special Interest Group)"]
    B[Open an issue, write a KEP, propose change to community]
    C[Discuss]
    D[Alpha]
    E[Discuss]
    F[Beta]
    G[Discuss]
    H[GA]
```

1. Discuss with SIG: Come to a general agreement about a feature
1. KEP
    - is this an enhancement?
    - Does it need a blog post?
    - Does it require communication with other SIGs?
    - Can it follow standard graudation route (alpha --> beta --> GA)
    - Does it redesign something?
    - Is it a big effort?
    - Does it impact user experience?
1. Discuss (A lot!)
1. Alpha
1. Discuss again!
1. Beta
1. Discuss Again!
1. GA

### Why so much discussion?

- Discussion with the community is the best way to get a feature implemented.
- Not advised to fork the project...K8s is hard to maintain. Very large and complicated codebase.

### What happens when things get stuck in discussion?

e.g. *Can we add colors to kubectl output?*

Discussion:
  - There are workarounds
  - Can colors impact scripts?
  - Can someone write themes?
  - Does this impact accessibility?
  - Is this worth the effort?

It happens. In some cases, the outcome may not be the simple/optimal solution for the initial feature request, but it should be valuable overall. e.g In this specific case, work has begun on implementing a `kuberc` file for configuring kubectl. This will allow for colored output but will also do more...

### Conclusions

- Features are hard. Lots and lots of discussion. K8s codebase is massive and complicated.
- Breaking changes are unacceptable. This slows things down...
- Sometimes people aren't willing to follow the whole process. The process is long and tiring, but it ultimately keeps K8s working.

### Takeaways

- Don't give up! K8s needs ideas and features. Worth the effort.
- Take a look into past enhancements. Explore features, track/test features in alpha. Provide feedback.
- Don't be shy.
- Not only code changes are needed/required. You can propose changes/improvements to docs or provide feedback as a user.

## 