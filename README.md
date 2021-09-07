# Ansible Playbook for Installing Hashicorp (only RAFT storage for now..)

* [Vault Deployment Guide](https://learn.hashicorp.com/tutorials/vault/raft-deployment-guide?in=vault/raft)
* [Vault Doc](https://www.vaultproject.io/docs/configuration/storage/raft)
## Supported OS

* RHEL/Centos 8
## Use with Vagrant

```bash
vagrant up
vagrant provision
```
## VMs details

Below a list of al VMs deloyed by Vagrant. 
We don't use port-forwading but we specify internal IPs.

|    VM     | Internal IP    |     Role       |
|-----------|----------------|----------------|
| vault01   |  192.168.58.71 |  Vault Node    |
| vault02   |  192.168.58.72 |  Vault Node    |
| vault03   |  192.168.58.73 |  Vault Node    |
| haproxy01 |  192.168.58.74 |  Reverse Proxy |

```bash
$ vagrant status
Current machine states:

vault01                   running (virtualbox)
vault02                   running (virtualbox)
vault03                   running (virtualbox)
haproxy01                 running (virtualbox)
```

## Create env and prepare TLS certificate

1. Create inventory file in inventories/. Example:  inventories/poste_cert.yml
2. Set "env" key into the inventory
3. Create TLS folder like provisioning/files/cert_tls (<env_name>_tls)
4. Put certificates and keys into provisioning/files/<env>_tls (ca.crt, tls.crt, tls.key)
## Init and Unseal Vault

```bash
export VAULT_CACERT=/opt/vault/tls/ca.crt
export VAULT_TLS_SERVER_NAME=<server_name>
vault status

vault operator init

# Store the unseal keys and the initial root token in a secure place.
vault operator unseal
```
