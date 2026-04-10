# Awesome Cloud Native [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of cloud native tools that are actually production-ready.

🍇 Curated by [Podo Stack](https://podostack.substack.com) — Ripe for Prod.

---

## Contents

- [Container Images](#-container-images)
- [Supply Chain & Runtime Security](#-supply-chain--runtime-security)
- [Autoscaling](#-autoscaling)
- [Networking & Service Mesh](#-networking--service-mesh)
- [Observability](#-observability)
- [GitOps](#-gitops)
- [Platform Engineering](#-platform-engineering)
- [Kyverno Policies](#-kyverno-policies)
- [CLI Tools & One-Liners](#-cli-tools--one-liners)

---

## 🖼️ Container Images

### Image Distribution & Caching

- **[Spegel](https://github.com/spegel-org/spegel)** - Nodes share container images directly with each other — no registry involved. Stateless P2P caching that speeds up scaling and cuts egress costs. `CNCF Sandbox`
  - 📖 [Deep dive](https://podostack.substack.com/p/spegel-pixie-and-why-latest-is-evil)

- **[Stargz Snapshotter](https://github.com/containerd/stargz-snapshotter)** - Start containers before the image fully downloads. Your app uses ~6% of files at startup — why pull 100%?
  - 📖 [Deep dive](https://podostack.substack.com/p/lazy-pull-smart-scale-ebpf-network)

---

## 🔐 Supply Chain & Runtime Security

- **[Trivy](https://github.com/aquasecurity/trivy)** - Single-binary CVE scanner for container images, IaC, and filesystems. No daemon, no config — drop it into CI and fail the build on criticals before they reach a registry.
  - 📖 [Deep dive](https://podostack.substack.com/p/signed-images-runtime-watchtowers-docker-pull-act-of-faith)

- **[cosign](https://github.com/sigstore/cosign)** - Signs container images like a wax seal on a letter — break the signature, everyone knows. Part of Sigstore, and keyless mode with GitHub OIDC means no private keys to manage. `Sigstore`
  - 📖 [Deep dive](https://podostack.substack.com/p/signed-images-runtime-watchtowers-docker-pull-act-of-faith)

- **[Falco](https://github.com/falcosecurity/falco)** - Watches Linux syscalls via eBPF while your containers run — every file open, every process spawn, every network connection. Real-time threat detection in the kernel, not "scan and report later". `CNCF Graduated`
  - 📖 [Deep dive](https://podostack.substack.com/p/signed-images-runtime-watchtowers-docker-pull-act-of-faith)
  - ⚔️ [Falco vs Tetragon](https://podostack.substack.com/p/ebpf-tetragon-parca-falco-sloth-alloy) — detection vs in-kernel enforcement

- **[Tetragon](https://github.com/cilium/tetragon)** - Doesn't just watch syscalls — it kills them with SIGKILL in the kernel before they complete. From the Cilium team, under 1% overhead, and Kubernetes-aware policies as CRDs instead of Lua scripts.
  - 📖 [Deep dive](https://podostack.substack.com/p/ebpf-tetragon-parca-falco-sloth-alloy)

- **[Chainguard Images](https://github.com/chainguard-images/images)** - Distroless-style base images with daily rebuilds and aggressive CVE tracking. About 5 CVEs per year instead of Alpine's 150 — nothing to exploit because there's almost nothing there.
  - 📖 [Deep dive](https://podostack.substack.com/p/signed-images-runtime-watchtowers-docker-pull-act-of-faith)

---

## ⚡ Autoscaling

- **[Karpenter](https://github.com/aws/karpenter)** - Provisions the exact node your pods need in seconds, not minutes. No node groups — just right-sized instances from any available type. `AWS` `GCP`
  - 📖 [Deep dive](https://podostack.substack.com/p/lazy-pull-smart-scale-ebpf-network)
  - 💰 [FinOps patterns](https://podostack.substack.com/p/spot-consolidation-pod-packing-40-percent-overpaying) — SpotToSpot consolidation, affinity traps, the knobs that actually move the bill

- **[Cluster Autoscaler](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler)** - The battle-tested autoscaler that works through Node Groups. Slower than Karpenter but multi-cloud and familiar.
  - 📖 [Deep dive](https://podostack.substack.com/p/lazy-pull-smart-scale-ebpf-network) — side-by-side with Karpenter

---

## 🌐 Networking & Service Mesh

- **[Cilium](https://github.com/cilium/cilium)** - Replaces kube-proxy with eBPF — O(1) lookups instead of walking iptables chains. Also does identity-based security and multi-cluster mesh. `CNCF Graduated`
  - 📖 [Deep dive](https://podostack.substack.com/p/cilium-ebpf-kube-proxy-identity-hubble) — kube-proxy replacement, Hubble, identity policies, egress gateway, cluster mesh

- **[Istio Ambient](https://github.com/istio/istio)** - Service mesh without sidecars. Uses a node-level ztunnel for L4 and on-demand waypoint proxies for L7 — pay only for what you need. `CNCF Graduated`
  - 📖 [Deep dive](https://podostack.substack.com/p/sidecar-free-mesh-slo-from-yaml-and)

---

## 📊 Observability

- **[Pixie](https://github.com/pixie-io/pixie)** - See your cluster's HTTP, SQL, and DNS traffic without touching your code. Uses eBPF to capture data — including decrypted TLS. `CNCF Sandbox`
  - 📖 [Deep dive](https://podostack.substack.com/p/spegel-pixie-and-why-latest-is-evil)

- **[sloth](https://github.com/slok/sloth)** - Define your SLOs in YAML, get Prometheus rules and Grafana dashboards. No more hand-rolling burn rate calculations.
  - 📖 [Deep dive](https://podostack.substack.com/p/sidecar-free-mesh-slo-from-yaml-and)

---

## 🚀 GitOps

- **[Flux](https://github.com/fluxcd/flux2)** - GitOps toolkit that actually waits for your deployments to be ready, not just applied. Handles Helm, Kustomize, and multi-tenancy. `CNCF Graduated`

---

## 🏗️ Platform Engineering

- **[Backstage](https://github.com/backstage/backstage)** - Software catalog that finally answers "who owns the payment service?" Developers drop a `catalog-info.yaml` next to their code, Backstage auto-discovers it, and the Scaffolder turns a three-day new-service bootstrap into three minutes. `CNCF Incubating`
  - 📖 [Deep dive](https://podostack.substack.com/p/guardrails-backstage-crossplane)

- **[Crossplane](https://github.com/crossplane/crossplane)** - Your cloud resources become Kubernetes objects with a continuous reconciliation loop. Someone edits an RDS instance by hand? Crossplane fixes it back — no Terraform drift, no CI wrapper. `CNCF Graduated`
  - 📖 [Deep dive](https://podostack.substack.com/p/crossplane-infrastructure-api-compositions-claims) — Compositions, Claims, Composition Functions, and the secret chain
  - 🧭 [Platform intro](https://podostack.substack.com/p/guardrails-backstage-crossplane) — Crossplane vs Terraform showdown

- **[Dapr](https://github.com/dapr/dapr)** - Sidecar that exposes distributed systems patterns — state, pub/sub, secrets, service invocation — through plain HTTP calls. Swap Redis for Postgres with a YAML change, not a code rewrite. `CNCF Incubating`
  - 📖 [Deep dive](https://podostack.substack.com/p/dapr-kargo-wasmedge-koordinator-openfeature)

- **[Kargo](https://github.com/akuity/kargo)** - Continuous promotion engine from the Argo CD team. Warehouse → Freight → Stage → Promotion makes GitOps promotion declarative instead of a shell script that rewrites `values.yaml`.
  - 📖 [Deep dive](https://podostack.substack.com/p/dapr-kargo-wasmedge-koordinator-openfeature)

- **[OpenFeature](https://github.com/open-feature)** - Does for feature flags what OpenTelemetry did for observability. One API, swap LaunchDarkly for Flagsmith with a single line — the hundreds of flag checks scattered across your codebase stay untouched. `CNCF Incubating`
  - 📖 [Deep dive](https://podostack.substack.com/p/dapr-kargo-wasmedge-koordinator-openfeature)

---

## 📜 Kyverno Policies

Write Kubernetes policies in plain YAML — no Rego, no new language to learn. [Kyverno](https://github.com/kyverno/kyverno) validates, mutates, and generates resources through admission webhooks. `CNCF Graduated`

- **[Disallow :latest tag](https://podostack.substack.com/i/185064206/the-policy-disallow-latest-tags)** — The `:latest` tag is mutable — the image it points to can change anytime. This policy blocks containers without explicit tags, preventing unpredictable deployments and rollback nightmares.

- **[Require labels](https://podostack.substack.com/i/185716285/the-policy-require-labels)** — Requires standard labels (`app.kubernetes.io/name`, `app.kubernetes.io/instance`) on Deployments, StatefulSets, and DaemonSets. Helps with cost allocation, automation, and keeping your cluster organized.

- **[Require PodDisruptionBudget](https://podostack.substack.com/i/187230155/the-policy-require-poddisruptionbudget)** — Node drain, three replicas, no PDB, all pods evicted at once, service down. This policy blocks any Deployment or StatefulSet with more than one replica that doesn't have a matching PDB.

---

## 🛠️ CLI Tools & One-Liners

### kubectl debug

Debug distroless containers by injecting ephemeral debug containers:

```bash
kubectl debug -it my-pod --image=busybox --target=my-container
```

### flux diff

"Terraform plan" for Kubernetes — see what would change before applying:

```bash
flux diff kustomization my-app --path ./clusters/prod/
```

### Karpenter drift detection

Check which nodes are marked for replacement:

```bash
kubectl get nodeclaims -o custom-columns=\
'NAME:.metadata.name,DRIFT:.status.conditions[?(@.type=="Drifted")].status'
```

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

Found a tool that should be here? [Open an issue](https://github.com/iGusev/awesome-cloud-native/issues/new?template=suggest-tool.md).

---

## License

[![CC0](https://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, the author has waived all copyright and related or neighboring rights to this work.
