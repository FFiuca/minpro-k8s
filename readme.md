- when spaming refresh in firts deploy (some pod has error), sometime got return error and success, it indicates browser hit different pod
-

How to add azure context and connect to your account :
1. az login
2. az aks create --name keyvault-demo-cluster -g keyvault-demo --node-count 1 --enable-addons azure-keyvault-secrets-provider --enable-oidc-issuer --enable-workload-identity  --generate-ssh-keys
3. az aks update -n keyvault-demo-cluster -g keyvault-demo --enable-disk-driver --enable-file-driver --enable-blob-driver --enable-snapshot-controller
4. go to aks dashboard, select then click connect
5. az account set --subscription c11b5f1e-a20b-4b57-b31e-3d91076a07b1
6. az aks get-credentials --resource-group keyvault-demo --name keyvault-demo-cluster --overwrite-existing
7. change your context docker to your aks name and kubectl command will use az aks

- it is look like database just use one statefulset and web apps connect to it. because when use more one replicas with same pvc, causing crash and locking file ibdata each pods look scrambfa-pull-left. By trial of django1 and django2 is not running well.
- when use multiple redis container. when store keys, it's not distribute completly. so for now use 1 redis instead multi
- and because master/slave logic is at application layer (need custom logic). use your cloud provider to achieve powefull apps. # https://chat.openai.com/c/35512008-66e9-4c02-bb6e-20fe3e15449d

- storageclass in local must enable csi-hostpath-driver minikube addons

Azure mysql
1. setting your firewall, whitelist ip, and ssl
2. download cert
3. use cert in your apps


AzPowerShell
https://learn.microsoft.com/en-us/powershell/azure/install-azps-windows?view=azps-11.5.0&tabs=windowspowershell&pivots=windows-psgallery
https://learn.microsoft.com/en-us/azure/mysql/flexible-server/connect-azure-cli#code-try-2
az mysql flexible-server connect -n django-ecommerce -u djangoecommerce01 -p "allinone-01" -d django_ecommerce --interactive
or
az mysql flexible-server connect -h django-ecommerce.mysql.database.azure.com -u djangoecommerce01 -p allinone-01 -d django_ecommerce
SET sql_generate_invisible_primary_key=OFF; or in server parameters, set it off
SELECT @@sql_generate_invisible_primary_key;

PVC
- When you specify a volume in a Deployment's pod template, like in your YAML where you define volume-django, it's the same volume shared across all replicas. In your case, all replicas of the Django application container will mount the same PVC (django-app-pv-claim) to access persistent storage.
