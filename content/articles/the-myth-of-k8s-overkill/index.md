---
title: "The Myth Of - You Don't Need Kubernetes"
date: 2026-05-09
draft: false
summary: "Don't Blame the Technology, Ask Questions Instead." 
showTaxonomies: true 
tags: ["k8s", "architecture", "rant"]
---

Most of the "Kubernetes is overkill" takes on LinkedIn are misleading.

A lot of the time, the people saying this were brought into companies where founders had burned through VC money on poor infrastructure decisions. Then comes the mandate to cut costs and justify the cleanup effort. The easiest punching bag becomes [Kubernetes](https://kubernetes.io/).

### Why?

Because managed Kubernetes offerings are expensive on paper. You pay for the control plane, then you pay again for the worker nodes underneath it.

But the comparison people make is rarely fair.

## 1. [ECS](https://aws.amazon.com/ecs/), [Cloud Run](https://cloud.google.com/run), etc. are only "simple" if your team already understands the ecosystem around them

Take ECS for example.

To deploy even a moderately production-ready application, you may end up setting up:

- An [ALB](https://aws.amazon.com/elasticloadbalancing/application-load-balancer/)
- Target groups
- IAM roles
- [Secrets Manager](https://aws.amazon.com/secrets-manager/) or [SSM](https://aws.amazon.com/systems-manager/)
- Service discovery
- Autoscaling policies
- Deployment automation wiring
- Separate integrations for observability and networking

A very simple example is scheduled jobs.

In Kubernetes, you define a [CronJob](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/) manifest.

That's it.

The cluster handles:

- Scheduling
- Retries
- Execution
- Cleanup
- Logging (uniform across the cluster once configured)
- Basic networking via your [CNI](https://github.com/containernetworking/cni)'s [NetworkPolicies](https://kubernetes.io/docs/concepts/services-networking/network-policies/)
- Advanced networking via a service mesh

All within the same API model as the rest of your workloads.

In ECS, scheduled workloads usually become:

- [EventBridge](https://aws.amazon.com/eventbridge/) schedules
- ECS RunTask targets
- IAM roles
- Networking config
- Logging config

Need conditional execution or chaining? Now [Step Functions](https://aws.amazon.com/step-functions/) or [Lambda](https://aws.amazon.com/lambda/) enters the picture.

None of this is "hard."

But operationally, it spreads one logical workflow across multiple AWS services.

Want progressive delivery pipelines?

Now you're wiring together:

- [CodeDeploy](https://aws.amazon.com/codedeploy/)
- ALB weighted target groups
- [CloudWatch](https://aws.amazon.com/cloudwatch/) alarms
- Deployment hooks
- Rollback logic

In Kubernetes, most of this becomes cluster-native abstractions:

- [Jobs](https://kubernetes.io/docs/concepts/workloads/controllers/job/)
- [Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
- [Probes](https://kubernetes.io/docs/concepts/configuration/liveness-readiness-startup-probes/)
- Rollout strategies
- [Argo Rollouts](https://argoproj.github.io/rollouts/) / [Flagger](https://fluxcd.io/flagger/)
- [Operators](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/)

The cluster itself becomes the deployment platform instead of stitching together multiple cloud services.

## 2. Kubernetes gives you one consistent model

You have:

- Deployments
- [Services](https://kubernetes.io/docs/concepts/services-networking/service/)
- [Ingress](https://kubernetes.io/docs/concepts/services-networking/ingress/) or [HTTPRoute](https://gateway-api.sigs.k8s.io/api-types/httproute/)
- [HPA](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) / [PDB](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/)
- CronJobs
- [StatefulSets](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)
- Operators for custom automation

Everything lives inside one control plane with a unified API model.

That consistency is the real selling point of Kubernetes, not just "containers at scale."

## 3. ASGs and Instance Groups put you back at square one

Another recommendation people throw around is [ASGs](https://docs.aws.amazon.com/autoscaling/ec2/userguide/auto-scaling-groups.html) or [Instance Groups](https://cloud.google.com/compute/docs/instance-groups).

At that point, we're back to square one: running applications directly on VMs.

One of the strongest reasons to adopt an orchestrator in the first place is to stop thinking about VM management all day:

- Better compute sharing
- Workload scheduling
- Self-healing
- Service abstraction
- Easier scaling semantics

If your answer is *"just use VMs,"* then ask whether you actually needed orchestration at all.

## 4. If you truly want a Kubernetes alternative, look at Nomad

If someone truly wants a Kubernetes alternative, [HashiCorp Nomad](https://developer.hashicorp.com/nomad) is probably the closest thing in terms of convenience and operational philosophy.

It's lighter operationally, supports non-containerized workloads, and feels far less overwhelming initially.

The biggest downside is ecosystem is not as vibrant and managed offerings are very few. Kubernetes won the ecosystem war long ago. 

In the HPC (High Performance Computing) world [SLURM](https://slurm.schedmd.com/overview.html), [OpenPBS](https://www.openpbs.org/) etc. are more popular and designed with HPC needs in mind. 

## So what am I implying here?

> Am I saying "always use Kubernetes"?

No.

> Am I saying "Kubernets is always easy"?

No.

What I'm saying is: choose what fits.

Ask:

- Do you even need an orchestrator?
- Does the extra control plane cost justify the sanity of your team?
- Is your team already comfortable with Kubernetes?
- Are you optimizing for portability, hiring, ecosystem, or simplicity?
- Is your scale operational complexity, or just traffic volume?
- How many services do you actually have?

Ask the right questions.

> Don't take constipation pills when you have diarrhea.

