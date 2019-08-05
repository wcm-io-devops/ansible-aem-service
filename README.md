# wcm_io_devops.aem_service

This role controls an Adobe Experience Manager (AEM) 6.x service on Linux servers and waits until the startup and shutdown is complete. It also provides a handler to only restart the instance as required from other roles/playbooks.

> This role was developed as part of the
> [wcm.io DevOps Ansible Automation for AEM](http://devops.wcm.io/ansible-aem/)
> to integrate Ansible with
> [CONGA](http://devops.wcm.io/conga/) but can be used independently of
> it.

## Requirements

This role requires Ansible 2.4 or higher and works with AEM 6.1 or
higher. The role requires an AEM service that can be controlled with the
Ansible `service` module to be installed on the target machine.

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

aem_service_restricted_mode: false

Enables / disables the restricted mode to work with customized commands like `sudo`.

    # aem_service_start_command: 

Overwrites the default (service manager related) start command.

    # aem_service_stop_command: 

Overwrites the default (service manager related) stop command.

    # aem_service_status_command: 

Overwrites the default (service manager related) status command.

    # aem_service_status_stopped_status_codes:

Overwrites the default (service manager related) stopped status codes
when set.

    # aem_service_status_started_status_codes:

Overwrites the default (service manager related) started status codes
when set.

    aem_service_status_valid_status_codes: "{{ _aem_service_status_stopped_status_codes | union(_aem_service_status_started_status_codes) | unique }}"
    
List of all valid AEM status codes.

## Dependencies

This role has no hard dependencies but interacts heavily with the [wcm_io_devops.aem_cms](https://github.com/wcm-io-devops/ansible-aem-cms) role.

## Example Playbook

Stops the `aem-author` instance and waits for the shutdown to complete: 

	- hosts: aem-author
	  roles:
	    - { role: wcm_io_devops.aem_service,
	        aem_service_state: stopped,
	        aem_service_name: aem-author,
	        aem_service_port: 4502 }

## License

Apache 2.0
