# Argo workflows scratch pad

This is an exercise to spike running a Docker-in-Docker workflow using a kind cluster

### Prerequisites

* Setup and run a [kind Kubernetes cluster](https://kind.sigs.k8s.io/)
* Use [asdf version manager](https://asdf-vm.com/)
* Run `asdf install` to install `argo`

### Run workflows

```bash
$ argo -n argo template create wft.yaml
$ argo -n argo submit wf.yaml --watch

# Workflow succeeds ✅
adam.barcan in dind on main 
aws:dev.use1(us-east-1) k8s:kind-local ❯ a get dind-test-48w96 
Name:                dind-test-48w96
Namespace:           argo
ServiceAccount:      default
Status:              Succeeded
Conditions:          
 PodRunning          False
 Completed           True
Created:             Wed Dec 01 12:29:41 -0500 (2 minutes ago)
Started:             Wed Dec 01 12:29:41 -0500 (2 minutes ago)
Finished:            Wed Dec 01 12:30:11 -0500 (1 minute ago)
Duration:            30 seconds
Progress:            2/2
ResourcesDuration:   35s*(1 cpu),35s*(100Mi memory)

STEP                 TEMPLATE      PODNAME                     DURATION  MESSAGE
 ✔ dind-test-48w96   entrypoint                                            
 ├───✔ hello         hello         dind-test-48w96-1018315872  6s          
 └───✔ dind-sidecar  dind-sidecar  dind-test-48w96-1746339360  12s         

# Able to execute a Docker command from sidecar ✅
$ argo -n argo logs 
aws:dev.use1(us-east-1) k8s:kind-local ❯ a logs dind-test-48w96 
dind-test-48w96-1018315872:  _____________ 
dind-test-48w96-1018315872: < Hello world >
dind-test-48w96-1018315872:  ------------- 
dind-test-48w96-1018315872:     \
dind-test-48w96-1018315872:      \
dind-test-48w96-1018315872:       \     
dind-test-48w96-1018315872:                     ##        .            
dind-test-48w96-1018315872:               ## ## ##       ==            
dind-test-48w96-1018315872:            ## ## ## ##      ===            
dind-test-48w96-1018315872:        /""""""""""""""""___/ ===        
dind-test-48w96-1018315872:   ~~~ {~~ ~~~~ ~~~ ~~~~ ~~ ~ /  ===- ~~~   
dind-test-48w96-1018315872:        \______ o          __/            
dind-test-48w96-1018315872:         \    \        __/             
dind-test-48w96-1018315872:           \____\______/   
dind-test-48w96-1746339360: Cannot connect to the Docker daemon at tcp://127.0.0.1:2375. Is the docker daemon running?
dind-test-48w96-1746339360: Unable to find image 'debian:latest' locally
dind-test-48w96-1746339360: latest: Pulling from library/debian
dind-test-48w96-1746339360: 647acf3d48c2: Pulling fs layer
dind-test-48w96-1746339360: 647acf3d48c2: Verifying Checksum
dind-test-48w96-1746339360: 647acf3d48c2: Download complete
dind-test-48w96-1746339360: 647acf3d48c2: Pull complete
dind-test-48w96-1746339360: Digest: sha256:e8c184b56a94db0947a9d51ec68f42ef5584442f20547fa3bd8cbd00203b2e7a
dind-test-48w96-1746339360: Status: Downloaded newer image for debian:latest
dind-test-48w96-1746339360: NAME="Alpine Linux"
dind-test-48w96-1746339360: ID=alpine
dind-test-48w96-1746339360: VERSION_ID=3.12.1
dind-test-48w96-1746339360: PRETTY_NAME="Alpine Linux v3.12"
dind-test-48w96-1746339360: HOME_URL="https://alpinelinux.org/"
dind-test-48w96-1746339360: BUG_REPORT_URL="https://bugs.alpinelinux.org/"
```
