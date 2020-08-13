# Mini HTCondor Infrastructure Deployment

Here we have a lonely Docker compose file that brings up [mini HTCondor](https://github.com/htcondor/htcondor/tree/master/build/docker/services) cm/schedd, worker and a submit node.

## Example Usage

Tested with docker-compose version `1.26.2`. 

1. Start all the services with docker-compose and make sure the containers are running:

```
~$ docker-compose up -d

~$ docker-compose ps

         Name                    Command           State           Ports         
---------------------------------------------------------------------------------
htc-mini-infra_schedd_1   /bin/bash -x /start.sh   Up      0.0.0.0:9618->9618/tcp
htc-mini-infra_submit_1   /bin/bash -x /start.sh   Up      9618/tcp              
htc-mini-infra_worker_1   /bin/bash -x /start.sh   Up
```

2. Login to the cm container and make sure that the worker(s) have been detected in the master. 

```
~$ docker ps | grep cm

16a32b83d825        htcondor/cm:8.9.6-el7        "/bin/bash -x /start…"   4 minutes ago       Up 4 minutes        0.0.0.0:9618->9618/tcp   htc-mini-infra_schedd_1

~$ docker exec -ti -u condor 16a32b83d825 bash

~$ bash-4.2$ condor_status
Name               OpSys      Arch   State     Activity LoadAv Mem   ActvtyTime

slot1@db5955c0137e LINUX      X86_64 Unclaimed Idle      0.000 1024  0+00:00:03

               Machines Owner Claimed Unclaimed Matched Preempting  Drain

  X86_64/LINUX        1     0       0         1       0          0      0

         Total        1     0       0         1       0          0      0

```

If you are too quick and do not see the workers: relax, take a break, make some tea and try again in a minute.


3. Submit a test job (mounted in /home/submituser/test.sub) from condor_submit container:

```
~$ docker ps | grep submit

fbe8cabde981        htcondor/submit:8.9.6-el7    "/bin/bash -x /start…"   7 minutes ago       Up 7 minutes        9618/tcp                 htc-mini-infra_submit_1

~$ docker exec -ti -u submituser fbe8cabde981 bash

~$ cd /home/submituser

~$ condor_submit test.sub

Submitting job(s).
1 job(s) submitted to cluster 1.

~$ condor_q

-- Schedd: fbe8cabde981 : <172.20.0.2:9618?... @ 08/13/20 09:54:36
OWNER      BATCH_NAME    SUBMITTED   DONE   RUN    IDLE  TOTAL JOB_IDS
submituser ID: 1        8/13 09:54      _      _      1      1 1.0

Total for query: 1 jobs; 0 completed, 0 removed, 1 idle, 0 running, 0 held, 0 suspended 
Total for submituser: 1 jobs; 0 completed, 0 removed, 1 idle, 0 running, 0 held, 0 suspended 
Total for all users: 1 jobs; 0 completed, 0 removed, 1 idle, 0 running, 0 held, 0 suspended
```

Now sit back and wait for the job to finish. The output for the test job you will find in the *.out file.
