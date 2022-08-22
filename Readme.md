# Netbox

## Description

Network and IP Adress Management

## Source

https://github.com/netbox-community/netbox-docker
https://github.com/CENGN/netbox-kubernetes



## Installation

### Setup Sealed Secrets

Generate all needed Secrets locally and seal them, so that you are able to store it in GIT. Just store the yaml Files with the sealed Secrets,
do not save your Real Passwords or the Secrets from Kubernetes, they are just Base64 Encoded, its easy to decode and therefore not safe!
For sealing you have to install Sealed Secrets from Bitnami. Find a tutorial with Kustomize and Argo CD in my repo [Sealed Secrets Kustomize](https://github.com/wep4you/sealed-secrets-kustomize) 

Now generate all needed Secrets replace <YOUR-PASSWORD> with a secure generated Password


    kubectl --namespace netbox-community create secret generic redis-secret --dry-run=client \
    --from-literal redis-password='<YOUR-PASSWORD>' \
    --output json | kubeseal | tee base/redis/sealedsecret.yml


    kubectl --namespace netbox-community create secret generic netbox-postgres-secret --dry-run=client \
    --from-literal postgres_password='<YOUR-PASSWORD>' \
    --output json | kubeseal | tee base/postgres/sealedsecret.yml


    kubectl --namespace netbox-community create secret generic netbox-secret --dry-run=client \
    --from-literal auth_ldap_bind_password='<YOUR-PASSWORD>' \
    --from-literal email_password='<YOUR-PASSWORD>' \
    --from-literal napalm_password='<YOUR-PASSWORD>' \
    --from-literal secret_key='<YOUR-PASSWORD>' \
    --from-literal superuser_password='<YOUR-PASSWORD>' \
    --from-literal superuser_api_token='<YOUR-PASSWORD>' \
    --from-literal superuser_email='<YOUR-PASSWORD>' \
    --from-literal superuser_name='<YOUR-PASSWORD>' \        
    --output json | kubeseal | tee base/netbox/sealedsecret.yml


#### Error on DB Initialization

If there is a problem with the init because the data dir is not empty, define a specific
directory for the database with Environment variable PGDATA

For example:
    PGDATA=/var/lib/postgresql/data/db-files/
