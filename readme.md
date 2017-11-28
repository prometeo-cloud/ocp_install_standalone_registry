# ocp_install_standalone_registry

## Description:

Install a standalone Docker registry running in a container.

## Behaviour:

**Feature:** Install the Standalone OpenShift Container Registry
As a System Admin
I want to install a Standalone OpenShift Container Registry
So that it can be used as a Master Registry to provide base images to a group of OCP clusters.

**Scenario:** Install OpenShift Container Registry (OCR)
- **Given** a RHEL machine has been configured as an OCP node
- **Given** Ansible is installed on the node
- **Given** Ansible OCP playbooks are installed on the node
- **Given** an Ansible inventory is available on the node to deploy the OCR
- **When** the installation is requested
- **Then** the OCR is deployed

## Configuration

### Variables

| Variable  | Description  | Default value |
|---|---|---|
| **registry_host_name** | The registry host name. | None |
| **master_default_subdomain** | The default sub-domain for the OCP master. | None |
| **min_memory** | Minimum memory in GB required by the OCR. | 16 |
| **min_cpus** | Minimum CPUs required by the OCR. | 2 |
| **ocp_version** | The version of OCP's OCR to install. | 3.6 |
| **repos_to_enable** | List of repositories to be enabled. | as described in Vars |
| **packages_to_install** | List of packages to be installed. | as described in Vars |

### LDAP Integration Variables

| Variable  | Description  | Example |
|---|---|---|
| **{{ customer_name }}** | The customer name with no blank spaces. | acme_corp |
| **{{ ldap_hostname }}** | The hostname of the LDAP host to be used by the registry | www.ldaphost.com |


