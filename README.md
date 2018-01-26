# aem-service

This role controls an Adobe Experience Manager (AEM) 6.x service on Linux servers and waits until the startup and shutdown is complete. It also provides a handler to only restart the instance as required from other roles/playbooks.
> This role was developed as part of the wcm.io set of roles to integrate Ansible with [CONGA](http://devops.wcm.io/conga/) and is designed to be used in combination with the [aem-cms](https://github.com/wcm-io-devops/ansible-aem-cms) role (but can be used independently). 

## Requirements

This role requires Ansible 2.0 or higher and works with AEM 6.1 or higher. The role requires an AEM service that can be controlled with the Ansible `service` module to be installed on the target machine.

## Role Variables

Available variables are listed below, along with their default values:

	aem_service_name: 

Name of the AEM service on the target machine. 

	aem_service_port:
 
To be able to poll for completion of the startup and shutdown process the role needs to know the port the AEM instance is listening on. 

Additionally the following optional variables are available:

	aem_service_state: started

The desired state of the service after this role finishes, one of `started`, `stopped` or `restarted`. `started` and `stopped` are idempotent and will not change the state unless necessary while `restarted` will always restart the service. 

	aem_service_timeout: 1200

The time to wait for the startup or shutdown to finish (in seconds).


## Dependencies

This role has no hard dependencies but interacts heavily with the [aem-cms](https://github.com/wcm-io-devops/ansible-aem-cms) role.

## Example Playbook

Stops the `aem-author` instance and waits for the shutdown to complete: 

	- hosts: aem-author
	  roles:
	    - { role: aem-service,
	        aem_service_state: stopped,
	        aem_service_name: aem-author,
	        aem_service_port: 4502 }

## License

Apache 2.0