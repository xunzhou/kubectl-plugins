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