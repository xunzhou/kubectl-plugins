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

recreate:  
kubectl recreate -f <file>

cordonx:
- kubectl cordonx <node1> [node2 ...]
- kubectl cordonx --undo

find:
- kubectl find <resource> <pattern1> [pattern2] [...]
- kubectl find po -i
- kubectl find po -s

vatt: 
- pvc to volumeattachment
- kubectl vatt <pvc_name_pattern>

grep:
- `kubectl get <resource> | grep <query>` shortcut; query matches any column
- kubectl grep <resource>[/<query>] [query]
- kubectl grep pod 7d                  (any column: NAME, STATUS, AGE, ...)
- kubectl grep 'pod*' nginx            (multi-resource via resource glob, parallel)
- kubectl grep '*/nginx'               (any resource, row contains 'nginx')
- -c / --compact: `<ns>\t<r>/<name>` per line (awk/xargs friendly)
- -nr / --not-ready: drop healthy rows (Completed|Succeeded|X/X)
- -E / --regex: treat query as raw regex (anchors, quantifiers)
- unknown flags pass through to kubectl get (-owide, --show-labels, ...)