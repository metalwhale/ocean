## Notes
- Charts in this directory were created by running:
  <pre>
  docker run --rm -v ${PWD}/:/apps alpine/helm:3.15.4 create <b>NAME</b>
  </pre>
  The version `3.15.4` was chosen to match [`helm_version` in Kubespray](https://github.com/kubernetes-sigs/kubespray/blob/v2.26.0/roles/kubespray-defaults/defaults/main/download.yml#L126)
