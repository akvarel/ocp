# ocp
## Prerequisites
### Ansible 2.9
On RedHat system enable repo:
````
subscription-manager repos --enable="rhel-7-server-ansible-2.9-rpms"
yum install ansible
````
This is repository for various ansible playbooks and roles required for day2 operation of OpenShift

Before you can use this collection you need to install roles and collections:
``ansible-galaxy install -r requirements.yml``

``inventory/group_vars/vars.yml`` contains default values for placement of backups and project names.

Create service account in a glusterfs project:
``oc create sa robot -n glusterfs``

Roles required for operation:
````
oc adm policy add-cluster-role-to-user cluster-admin system:serviceaccount:glusterfs:robot
````
``inventory/group_vars/api.yml`` contains encrypted by ansible-vault values for OpenShift master API endpoint and token from ``robot`` service account:
````
ansible-vault encrypt_string --stdin-name 'api_token'
ansible-vault encrypt_string --stdin-name 'master_api'
````
Important variables for backup of PV:
````
backup_dir_base
mount_point: "/mnt/source"
userheketi: admin
snapdir: "/root"
voldir: "/root"
vollist: "{{voldir}}/vollist-{{ lookup('pipe', 'date +%Y%m%d-%H:%M') }}.txt"
storage_namespace: glusterfs
````

If you need to create a project request template use this command and then customize the result:
````
oc adm create-bootstrap-project-template -o yaml > templates/project-request-template.yaml
````