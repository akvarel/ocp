# ocp
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
``inventory/group_vars/api.yml`` contains encrypted by ansible-vault values for OpenShift master API endpoint and token from ``robot`` service account.

Important variables for backup of PV:
````
backup_dir_base
mount_point: "/mnt/source"
userheketi: admin
snapdir: "/root"
voldir: "/root"
vollist: "{{voldir}}/vollist-{{ lookup('pipe', 'date +%Y%m%d-%H:%M') }}.txt"
````