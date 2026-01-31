# Awesome Cloud Native [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of cloud native tools that are actually production-ready.

🍇 Curated by [Podo Stack](https://podostack.substack.com) — Ripe for Prod.

---

## Contents

- [Container Images](#-container-images)
- [Autoscaling](#-autoscaling)
- [Networking & Service Mesh](#-networking--service-mesh)
- [Observability](#-observability)
- [Policy & Security](#-policy--security)
- [GitOps](#-gitops)
- [CLI Tools & One-Liners](#-cli-tools--one-liners)

---

## 🖼️ Container Images

### Image Distribution & Caching

- **[Spegel](https://github.com/spegel-org/spegel)** - Nodes share container images directly with each other — no registry involved. Stateless P2P caching that speeds up scaling and cuts egress costs. `CNCF Sandbox`
  - 📖 [Deep dive](https://podostack.substack.com/p/spegel-pixie-and-why-latest-is-evil)

- **[Stargz Snapshotter](https://github.com/containerd/stargz-snapshotter)** - Start containers before the image fully downloads. Your app uses ~6% of files at startup — why pull 100%?
  - 📖 [Deep dive](https://podostack.substack.com/p/lazy-pull-smart-scale-ebpf-network)

---

## ⚡ Autoscaling

- **[Karpenter](https://github.com/aws/karpenter)** - Provisions the exact node your pods need in seconds, not minutes. No node groups — just right-sized instances from any available type. `AWS` `GCP`
  - 📖 [Deep dive](https://podostack.substack.com/p/lazy-pull-smart-scale-ebpf-network)

- **[Cluster Autoscaler](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler)** - The battle-tested autoscaler that works through Node Groups. Slower than Karpenter but multi-cloud and familiar.
  - 📖 [Comparison with Karpenter](https://podostack.substack.com/p/lazy-pull-smart-scale-ebpf-network)

---

## 🌐 Networking & Service Mesh

- **[Cilium](https://github.com/cilium/cilium)** - Replaces kube-proxy with eBPF — O(1) lookups instead of walking iptables chains. Also does identity-based security and multi-cluster mesh. `CNCF Graduated`
  - 📖 [Deep dive](https://podostack.substack.com/p/lazy-pull-smart-scale-ebpf-network)

- **[Istio Ambient](https://github.com/istio/istio)** - Service mesh without sidecars. Uses a node-level ztunnel for L4 and on-demand waypoint proxies for L7 — pay only for what you need. `CNCF Graduated`
  - 📖 [Deep dive](https://podostack.substack.com/p/sidecar-free-mesh-slo-from-yaml)

---

## 📊 Observability

- **[Pixie](https://github.com/pixie-io/pixie)** - See your cluster's HTTP, SQL, and DNS traffic without touching your code. Uses eBPF to capture data — including decrypted TLS. `CNCF Sandbox`
  - 📖 [Deep dive](https://podostack.substack.com/p/spegel-pixie-and-why-latest-is-evil)

- **[sloth](https://github.com/slok/sloth)** - Define your SLOs in YAML, get Prometheus rules and Grafana dashboards. No more hand-rolling burn rate calculations.
  - 📖 [Deep dive](https://podostack.substack.com/p/sidecar-free-mesh-slo-from-yaml)

---

## 🔒 Policy & Security

- **[Kyverno](https://github.com/kyverno/kyverno)** - Kubernetes-native policy engine. Validate, mutate, and generate resources with simple YAML — no new language to learn. `CNCF Incubating`
  - 📖 [Disallow :latest tags](https://podostack.substack.com/p/spegel-pixie-and-why-latest-is-evil)
  - 📖 [Require labels](https://podostack.substack.com/p/sidecar-free-mesh-slo-from-yaml)

---

## 🚀 GitOps

- **[Flux](https://github.com/fluxcd/flux2)** - GitOps toolkit that actually waits for your deployments to be ready, not just applied. Handles Helm, Kustomize, and multi-tenancy. `CNCF Graduated`
  - 📖 [flux diff one-liner](https://podostack.substack.com/p/spegel-pixie-and-why-latest-is-evil)

---

## 🛠️ CLI Tools & One-Liners

### kubectl debug

Debug distroless containers by injecting ephemeral debug containers:

```bash
kubectl debug -it my-pod --image=busybox --target=my-container
```

- 📖 [Deep dive](https://podostack.substack.com/p/sidecar-free-mesh-slo-from-yaml)

### flux diff

"Terraform plan" for Kubernetes — see what would change before applying:

```bash
flux diff kustomization my-app --path ./clusters/prod/
```

- 📖 [Deep dive](https://podostack.substack.com/p/spegel-pixie-and-why-latest-is-evil)

### Karpenter drift detection

Check which nodes are marked for replacement:

```bash
kubectl get nodeclaims -o custom-columns=\
'NAME:.metadata.name,DRIFT:.status.conditions[?(@.type=="Drifted")].status'
```

- 📖 [Deep dive](https://podostack.substack.com/p/lazy-pull-smart-scale-ebpf-network)

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

Found a tool that should be here? [Open an issue](https://github.com/iGusev/awesome-cloud-native/issues/new?template=suggest-tool.md).

---

## License

[![CC0](https://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, the author has waived all copyright and related or neighboring rights to this work.
