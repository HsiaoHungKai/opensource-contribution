# Ray Report

### Contribution

- I track the Ray issue "Rewrite ThreadedRayInstaller to Use Asyncio". It comes from a TODO at `autoscaler.py:167`. I traced the install flow, from the autoscaler tick to the Ray install.
- I reviewed ray-project/ray#65171. The PR removes `_compute_min_resource_demand`, which `_try_schedule` uses.
- I reviewed ray-project/ray#65763. The PR adds `idleTerminationOptions` support to RayCluster. It merged.

### Learn

- I understand the autoscaler tick. `Autoscaler.update_autoscaling_state` reads the cluster state and calls `Reconciler.reconcile`. This method has two phases. First it syncs state from the cloud provider, the Ray cluster and the installers, with no side effects. Then it steps each instance forward.
- I understand the install path. `Reconciler._install_ray` sends RAY_INSTALLING events for allocated, running, non-head instances to `InstanceManager.update_instance_manager_state`. That method checks the expected version, writes to instance storage, and notifies every subscriber. This is how `ThreadedRayInstaller` starts.
- I understand the versions. The storage keeps one version counter for the whole instance table. Each instance keeps the storage version of its last write. A mismatch returns VERSION_MISMATCH and writes nothing.
