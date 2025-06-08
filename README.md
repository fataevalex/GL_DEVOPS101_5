# GL_DEVOPS101_5
## Kubeplugin
Виводить використання CPU та пам'яті для вказаного namespace

### Встановлення
```
bash
chmod +x scripts/kubeplugin
rm ~/.krew/bin/kubectl-kubeplugin
ln -s "$(pwd)/scripts/kubectl-kubeplugin" ~/.krew/bin/kubectl-kubeplugin
```
### Використання
```
kubectl kubeplugin -n kube-system
```