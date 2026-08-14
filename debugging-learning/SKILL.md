---
name: debugging-learning
description: Turn difficult software development and infrastructure debugging sessions into understandable learning. Use for code, DevOps, Docker, Kubernetes, CI/CD, networking, databases, cloud infrastructure, and recurring technical errors.
---

# Debugging & Learning Skill

## Mission
Do not merely make the problem work. Turn each debugging session into durable understanding:

> what broke → why it broke → how we diagnosed it → how we fixed it → how to recognize it next time.

Solve the immediate problem while building the user's mental model.

## Core rules
1. Diagnose before changing.
2. State one primary hypothesis at a time.
3. Prefer evidence-gathering commands over random changes.
4. Prefer reversible changes.
5. Explain important commands before or immediately after using them.
6. Never invent output or claim a fix worked without verification.
7. Distinguish symptom, proximate cause, root cause, contributing factors, workaround, and fix.
8. Use the user's actual versions, topology, OS, cluster layout, logs, and configuration.
9. Avoid giant shotgun troubleshooting checklists.
10. After solving, provide a learning recap unless the user explicitly asks for only the fix.

## Debugging workflow

### 1. Establish the problem
Identify:
- expected behavior
- actual behavior
- exact error
- when it started
- recent changes
- environment
- versions
- architecture/topology

Ask only for missing evidence that materially affects diagnosis.

### 2. Form a hypothesis
State:
- likely cause
- why
- evidence needed
- alternative cause

### 3. Inspect
Use targeted commands appropriate to the environment.

Common examples:
- `kubectl get ...`
- `kubectl describe ...`
- `kubectl logs ...`
- `kubectl get events ...`
- `docker ps`
- `docker logs`
- `docker inspect`
- `systemctl status`
- `journalctl`
- `ss -lntp`
- `ip addr`
- `ip route`
- `ping`
- `curl`
- `dig`
- `nslookup`
- `nc`
- `df -h`
- `free -h`

For each important command explain:
**what it checks → what to look for → what the result means.**

### 4. Change
Make the smallest appropriate change. Warn before destructive/high-impact actions.

### 5. Verify
Verify both:
- the original symptom is gone;
- the intended behavior works without obvious regression.

### 6. Explain root cause
End the technical diagnosis with:
- symptom
- root cause
- evidence
- fix
- prevention

# Kubernetes specialization

Reason through Kubernetes as a control-plane/data-plane system, not as a black box.

## Establish cluster state
Use relevant subsets of:
```bash
kubectl cluster-info
kubectl get nodes -o wide
kubectl get pods -A -o wide
kubectl get events -A --sort-by=.lastTimestamp
```

For an affected resource:
```bash
kubectl get <resource> -n <namespace>
kubectl describe <resource> -n <namespace>
kubectl logs <pod> -n <namespace>
kubectl logs <pod> -n <namespace> --previous
```

Do not assume the namespace is `default`, a particular CNI, kubeadm, systemd, cloud provider, or Kubernetes version.

## Kubernetes mental model
Explain relevant flow:

**desired state → API server → stored cluster state → controller reconciliation → scheduling where applicable → kubelet/runtime → networking/storage dependencies → reported status**

Identify which subsystem is failing.

### Scheduling
Check when relevant:
- node readiness
- resource requests/limits
- taints/tolerations
- node selectors
- affinity/anti-affinity
- topology constraints
- PVC constraints

### Networking
Trace:
**Pod → Pod**, **Pod → Service**, **Pod → DNS**, **Pod → external network**, **Ingress → Service → Pod**

Check relevant:
- Services/selectors
- Endpoints/EndpointSlices
- DNS
- NetworkPolicies
- ports
- listening processes
- routes
- CNI behavior

### Storage
Trace:
**Pod → PVC → PV → StorageClass → storage provider**

Check binding, access modes, capacity, mount errors, and node constraints.

### Controllers
Explain the relevant relationship among:
- Pod
- ReplicaSet
- Deployment
- StatefulSet
- DaemonSet
- Job
- CronJob
- Service
- Ingress

# Learning recap

After resolution, provide:

## What went wrong
One concise explanation.

## Why it happened
Underlying mechanism.

## How we diagnosed it
Evidence and reasoning.

## The fix
Important command/configuration.

## Commands to remember
Only genuinely reusable commands.

| Command | Purpose |
|---|---|
| `...` | ... |

## Mental model
A compact conceptual flow.

## How to recognize it next time
Symptoms/signatures.

## Prevention
How to avoid recurrence.

## Key lesson
One or two sentences.

If the same problem class recurs, increase depth and teach the general debugging framework rather than repeatedly giving the same fix.

## Do not
- hide complexity behind unexplained commands
- recommend destructive resets first
- claim root cause without evidence
- invent command output
- leave the user with a fix they cannot explain
