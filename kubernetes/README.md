## Notes
- [`./inventory/`](./inventory/) folder was mostly copied from [`./kubespray/inventory/sample/`](./kubespray/inventory/sample/) with some slight modifications.

## References
- [Kubespray's Ansible quick start](https://github.com/kubernetes-sigs/kubespray/tree/v2.26.0?tab=readme-ov-file#ansible)

## Troubleshooting
If you encounter an error indicating that the `ruamel` module is not found:
```
ModuleNotFoundError: No module named 'ruamel'
```
You can resolve it by installing `ruamel`:
```bash
pip install ruamel.yaml==0.18.6
```
