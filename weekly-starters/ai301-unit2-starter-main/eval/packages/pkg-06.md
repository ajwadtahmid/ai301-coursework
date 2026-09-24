# Eval package: pkg-06

- source: kubernetes/minikube#11645
- captured: 2026-08-17
- calibration: false

## Repo facts (captured 2026-08-17)

- repo: kubernetes/minikube (32039 stars, archived: no)
- description: Run Kubernetes locally.
- latest release: v1.38.1 (2026-02-19)
- bug reports: template asks what happened, the exact command to reproduce, the full output of the failed command, the output of `minikube logs`, the operating system, and the driver
- contribution policy (CONTRIBUTING.md): standard Kubernetes-project contribution guide (CLA, DCO); no stated AI policy

## Issue

### Minikube tunnel is not working on Windows (endessly trying to add a route) (#11645)

opened by sm4sh1k (NONE) on 2021-06-13, state open, labels: kind/bug, help wanted, priority/important-longterm, lifecycle/frozen, good first issue, area/tunnel

Minikube tunnel is not working on Windows because minikube can not recognize the correct answer (or status code) of the **route** command.

Steps to reproduce the issue:

1. `minikube start --driver vmware`
2. `minikube tunnel --cleanup --alsologtostderr`

The full output shows the tunnel loop repeatedly logging `About to run cmd: route ... ADD` and retrying: minikube keeps re-adding the same route because it misreads the Windows `route` command's output, so the tunnel never settles.

## Thread highlights (15 comments total)

- 2021-07-18 joseluisgomes (NONE): currently has the same problem
- 2021-09-09 ziemerz (NONE): facing similar issues on Mac with the hyperkit driver
- 2021-10-08 Nirvandil (NONE): same with the VirtualBox backend

## Candidate claim comment

I'd like to work on this tunnel issue as my first contribution here. I have it reproducing locally (report below) and plan to look at how the tunnel manager parses the `route` command's output on Windows.

## Candidate repro report

Reproduced the endless route-add loop. Steps:

1. `minikube start`
2. `minikube tunnel --alsologtostderr`
3. Watch the log output loop.

Output excerpt from step 2:

```
I0812 14:03:11.402181    9184 out.go:296] Setting JSON to false
I0812 14:03:14.117290    9184 tunnel.go:58] Checking for tunnels to cleanup...
I0812 14:03:14.883104    9184 loadbalancer_patcher.go:80] Patched svc/hello-node with an external IP
I0812 14:03:15.021447    9184 route.go:118] About to run cmd: route ADD 10.96.0.0 MASK 255.240.0.0 192.168.59.101
I0812 14:03:16.339812    9184 route.go:130] error adding route: exit status 1, retrying
I0812 14:03:17.481533    9184 route.go:118] About to run cmd: route ADD 10.96.0.0 MASK 255.240.0.0 192.168.59.101
I0812 14:03:18.612204    9184 route.go:130] error adding route: exit status 1, retrying
I0812 14:03:19.744981    9184 route.go:118] About to run cmd: route ADD 10.96.0.0 MASK 255.240.0.0 192.168.59.101
```

Expected: the route is added once and `minikube tunnel` reports the tunnel as running.

Actual: the same `route ADD` line repeats indefinitely with `error adding route: exit status 1, retrying` between repetitions, as shown above, and the tunnel never becomes ready.
