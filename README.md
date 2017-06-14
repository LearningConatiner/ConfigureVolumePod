# ConfigureVolumePod
1. Create a Pod

srilakshmi_krishnamoorthy@x-jigsaw-168411:~/kubernetesex/volumeex$ kubectl create -f pod-redis.yaml
pod "redis" created

2.Watch the pod
srilakshmi_krishnamoorthy@x-jigsaw-168411:~/kubernetesex/volumeex$ kubectl get --watch pod redis
NAME      READY     STATUS    RESTARTS   AGE
redis     1/1       Running   0          55s

3.In another terminal, get a shell to the running Container:
 kubectl exec -it redis -- /bin/bash
In your shell, go to /data/redis, and create a file:
 root@redis:/data/redis# echo Hello > test-file
In your shell, list the running processes:
 root@redis:/data/redis# ps aux
 
 kubectl exec -it redis -- /bin/bash
root@redis:/data# cd /data/redis                                                                                                                                 
root@redis:/data/redis# echo Hello > test-file
root@redis:/data/redis# ps aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
redis        1  0.1  0.1  33312  3876 ?        Ssl  14:04   0:00 redis-server *:6379
root        12  0.0  0.0  20228  3208 ?        Ss   14:06   0:00 /bin/bash
root        16  0.0  0.0  17500  2108 ?        R+   14:06   0:00 ps aux

In your shell, kill the redis process:
 root@redis:/data/redis# kill <pid>
 
 root@redis:/data/redis# kill 1
root@redis:/data/redis# srilakshmi_krishnamoorthy@x-jigsaw-168411:~/kubernetesex/projectedvolumeex$ 

In your original terminal, watch for changes to the redis Pod. Eventually, you will see something like this

srilakshmi_krishnamoorthy@x-jigsaw-168411:~/kubernetesex/volumeex$ kubectl get --watch pod redis
NAME      READY     STATUS    RESTARTS   AGE
redis     1/1       Running   0          55s
redis     0/1       Completed   0         3m
redis     1/1       Running   1         3m

At this point, the Container has terminated and restarted. This is because the redis Pod has a restartPolicy of Always.
Get a shell into the restarted Container:
 kubectl exec -it redis -- /bin/bash
In your shell, goto /data/redis, and verify that test-file is still there.
What’s next
srilakshmi_krishnamoorthy@x-jigsaw-168411:~/kubernetesex/projectedvolumeex$ kubectl exec -it redis -- /bin/bash
root@redis:/data# cd /data/redis                                                                                                                                 
root@redis:/data/redis# ls
test-file
root@redis:/data/redis# 
