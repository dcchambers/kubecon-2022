# Session Notes and Thoughts

## Contents

- [Learn About Helm and its Ecosystem](#learn-about-helm-and-its-ecosystem)
- [Prometheus - Intro, Deep Dive, And Open Q+A](#prometheus---intro-deep-dive-and-open-qa)
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

## Prometheus - Intro, Deep Dive, And Open Q+A
- **Title**: Prometheus - Intro, Deep Dive, And Open Q+A
- **Presenters**: Goutham Veeramachaneni & Ganesh Vernekar, Grafana Labs

Prometheus: A metrics-based monitoring & alerting stack.
- Instrumentation for apps and systems
- Metrics collection and storage
- Querying, alerting, dashboards
- Designed from the ground up for dynamic cloud environemnts.

What is it not?
- logging or tracing
- automatic anomaly detection
- scalable or durable storage

### History

- Started at SoundCloud in 2012 from some ex-Googlers that missed Google's monitoring systems.
- Made public in 2015.
- Joined CNCF in 2016.

### Architecture

- Prometheus uses a pull-based system to gather metrics/data.
- Include Prometheus client libraries in your application.
  - E.g. include a /metrics API endpoint that prometheus can collect data from. 
- Prometheus collects data from various targets (API endpoints, VM, cgroups, etc) and stores them in central database for collection/storage/processing
- Prometheus neends to know what services exist.
  - It communicates with service discovery (DNS, K8s, AWS, Consul, etc)
  - Because Prometheus talks to your service discovery/k8s API in a pull-based manor, it can tell when services are down (or if only certain percent of services are down)
- UI
  - Web UI
  - Grafana
  - Automation
- Alerting
  - Alertmanager

### Selling Points

- Data model
- Query Language (PromQL)
- Simple & Efficient Server
- Service Discovery Integration

### Data Model

- A label-based time series

### Querying

- PromQL Query Language
  - Functional
  - Not SQL-style
  - Support for custom groupings
  - Supports time offsets

Simple example: What is the ratio of request errors across all service instances?
- `sum(rate(http_requests_total{status="500"}[5m])) / sum(rate(http_requetss_total[5m]))`

Group by path:
- `sum by(path) (rate(http_requests_total{status="500"}[5m])) / sum by(path) (rate(http_requetss_total[5m]))`

### Alert

Configure alerts based on a matching time series

### Efficiency

Local storage is scalable enough for many orgs.
- 1M+ samples/sec
- Millions of series
- 1-2 bytes per sample
- Good for keeping weeks ro months of data, and some orgs keep years with careful backup processes.

### Not Everything "Speaks" Prometheus
- Prometheus exporters - many are available!
    - Translate from other metric systems
    - Transform system-specific metrics
    - Do it yourself (JSON exporter, Python, Go, etc)

### Remote Write Receiver
Configure prometheus to write data to a remote server.

### Agent Mode
If you don't want to store any data in prometheus.
- Forwards data to remote storage.
- Reduces load/server requirements.

### Release Cycle
- Up until recently, new version released every 6 weeks.
- Starting in v2.37, LTS support added (6+ months)
  - Will get backports of critical bug fixes.

### New/What's Coming
- Support for out-of-order time series (v2.39)
- Support for native histograms (v2.40)

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