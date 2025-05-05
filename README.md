cfgmgr:  
- Merge/Delete kubeconfig file to ~/.kube/config 
 
secrets:
- Base64 decode secrets data

entry:
- kubectl exec bash/sh in container
- `kubectl entry <fzf query term> [-n namespace] [-c container]`

log:
- kubectl logs with fzf
- `kubectl log <fzf query term> [-f] [-p] [-n namespace] [-c container] ...`

desc:
- kubectl describe with fzf
- `kubectl desc [resource] [fzf query term]`

g: 
- kubectl get with fzf
- `kubectl desc [resource] [fzf query term] [options]`

diff-rs:
- kubectl diff-rs <replicaset>
- kubectl diff-rs <replicaset> LAST LAST-3