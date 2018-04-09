# Limit a container's resources

By default, a container has no resource constraints and can use as much of a given resource as the host’s kernel scheduler allows. Docker provides ways to control how much memory, CPU, or block IO a container can use, setting runtime configuration flags of the docker run command. 

## Limit a container’s access to memory
Docker can enforce hard memory limits, which allow the container to use no more than a given amount of user or system memory, or soft limits, which allow the container to use as much memory as it needs unless certain conditions are met, such as when the kernel detects low memory or contention on the host machine. Some of these options have different effects when used alone or when more than one option is set.

| Option |	Description |
|---|---|
|`-m` or `--memory=` |	The maximum amount of memory the container can use. If you set this option, the minimum allowed value is 4m (4 megabyte). |
|--memory-swap*	| The amount of memory this container is allowed to swap to disk |


`--memory-swap` is a modifier flag that only has meaning if `--memory` is also set. Using swap allows the container to write excess memory requirements to disk when the container has exhausted all the RAM that is available to it. There is a performance penalty for applications that swap memory to disk often.

Its setting can have complicated effects:

If `--memory-swap` is set to a positive integer, then both `--memory` and `--memory-swap` must be set. `--memory-swap` represents the total amount of memory and swap that can be used, and --memory controls the amount used by non-swap memory. So if `--memory="300m"` and `--memory-swap="1g"`, the container can use 300m of memory and 700m (1g - 300m) swap.

If `--memory-swap` is set to 0, the setting is ignored, and the value is treated as unset.

If `--memory-swap` is set to the same value as --memory, and --memory is set to a positive integer, the container does not have access to swap. 

## CPU
By default, each container’s access to the host machine’s CPU cycles is unlimited. You can set various constraints to limit a given container’s access to the host machine’s CPU cycles. Most users use and configure the default CFS scheduler. In Docker 1.13 and higher, you can also configure the realtime scheduler.

| Option |	Description |
|---|---|
| `--cpus=<value>` |	Specify how much of the available CPU resources a container can use. For instance, if the host machine has two CPUs and you set --cpus="1.5", the container is guaranteed at most one and a half of the CPUs. |
| `--cpu-period=<value>` |	Specify the CPU CFS scheduler period, which is used alongside --cpu-quota. Defaults to 100 micro-seconds.  |
| `--cpu-quota=<value>` |	Impose a CPU CFS quota on the container. The number of microseconds per --cpu-period that the container is guaranteed CPU access. |


```
docker run -it --cpus=".5" ubuntu /bin/bash
```

