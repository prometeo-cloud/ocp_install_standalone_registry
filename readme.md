# ocp_install_standalone_registry

## Description:

Install a standalone Docker registry running in a container.

## Behaviour:

**Feature:** Install the Standalone OpenShift Container Registry

**As a** System Admin  
**I want to** install a Standalone OpenShift Container Registry  
**So that** it can be used as a Master Registry to provide base images to a group of OCP clusters.

**Scenario:** Install OpenShift Container Registry (OCR)
- **Given** a RHEL machine has been configured as an OCP node
- **Given** Ansible is installed on the node
- **Given** Ansible OCP playbooks are installed on the node
- **Given** an Ansible inventory is available on the node to deploy the OCR
- **When** the installation is requested
- **Then** the OCR is deployed

## Configuration

### Connection Variables

The following variables should be provided to establish a connection to the remote hosts:

| Variable  | Description  | Example |
|---|---|---|
|**ocp_ansible_ssh_user**| The name of the sudo user used to connect to the remote host. | "vagrant" |
|**ocp_ansible_ssh_private_key_file**| The location of the private key to use to connect to the remote host. | "~/.ssh/private_key"|

### General Variables

| Variable  | Description  | Default value |
|---|---|---|
| **registry_host_names** | The list of host names for the registry. | None |
| **master_default_subdomain** | The default sub-domain for the OCP master. | None |
| **repos_to_enable** | List of repositories to be enabled. | as described in Vars |
| **packages_to_install** | List of packages to be installed. | as described in Vars |
| **openshift_ansible_version** | The openshift-ansible github project release tag.  | release-3.6 |

### No-Authentication

**NOTE**: Requires setting **id_provider_type** variable to an empty string.

### HTPasswd authentication

**NOTE**: Requires setting **id_provider_type** variable to "HTPasswd".

| Variable  | Description  | Default |
|---|---|---|
| **htpasswd_filename** | The location of the htpasswd file to be used. | /etc/origin/master/htpasswd |

### LDAP Integration Variables

**NOTE**: Requires setting **id_provider_type** variable to 'LDAP'.

| Variable  | Description  | Example | Mandatory |
|---|---|---|---|
| **ldap_hostnames** | The hostname(s) of the LDAP host to be used by the registry | www.ldaphost.com | yes |
| **ldap_url** | The URL of the LDAP server. |  ldap://www.example.com/ou=users,dc=acme,dc=com?uid | yes |
| **ldap_certificate_file** | Certificate bundle to use to validate server certificates for the configured URL. If empty, system trusted roots are used. Only applies if insecure: false. | ldap-ca-bundle.crt (defaults to an empty string)| no |
| **ldap_insecure** | When true, no TLS connection is made to the server. When false, ldaps:// URLs connect using TLS, and ldap:// URLs are upgraded to TLS. | true (defaults to false) | no |

## Testing

To set up a local ldap server to test the integration:

```bash
# starts a docker container running open-ldap
$ docker run --name open_ldap --hostname localhost --detach -p 389:389 -p 636:636 osixia/openldap:1.1.10

# running a query
ldapsearch -x -H ldap://localhost -b dc=example,dc=org -D "cn=admin,dc=example,dc=org" -w admin

# starts the admin interface
docker run -p 6443:443 --name open_ldap_admin --env PHPLDAPADMIN_LDAP_HOSTS=localhost --detach osixia/phpldapadmin:0.7.0

```
