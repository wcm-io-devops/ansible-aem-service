# wcm_io_devops.aem_service

This role controls an Adobe Experience Manager (AEM) 6.x service on Linux servers and waits until the startup and shutdown is complete. It also provides a handler to only restart the instance as required from other roles/playbooks.

> This role was developed as part of the
> [wcm.io DevOps Ansible Automation for AEM](http://devops.wcm.io/ansible-aem/)
> to integrate Ansible with
> [CONGA](http://devops.wcm.io/conga/) but can be used independently of
> it.

## Requirements

This role requires Ansible 2.0 or higher and works with AEM 6.1 or higher. The role should be used with an AEM service that can be controlled with the Ansible `service` module on the target machine. Alternatively, raw commands for stopping and starting can be utilised.

## Role Variables

Available variables are listed below, along with their default values:

	aem_service_name: 

Name of the AEM service on the target machine. 

	aem_service_port:
 
To be able to poll for completion of the startup and shutdown process the role needs to know the port the AEM instance is listening on. 

Additionally the following optional variables are available:

	aem_service_state: started

The desired state of the service after this role finishes, one of `started`, `stopped` or `restarted`. `started` and `stopped` are idempotent and will not change the state unless necessary while `restarted` will always restart the service.
When using  `aem_service_raw_cmd_start` or `aem_service_raw_cmd_stop`, idempotency depends on the behaviour of the command line executed.

	aem_service_timeout: 1200

The time to wait for the startup or shutdown to finish (in seconds).

    aem_service_raw_cmd_start:

Optional start command line to use instead of the ansible service module.

    aem_service_raw_cmd_stop:

Optional stop command line to use instead of the ansible service module.


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

Stops the `aem-author` instance using a custom raw command line and waits for the restart to complete:

	- hosts: aem-author
	  roles:
	    - { role: wcm_io_devops.aem_service,
	        aem_service_state: restarted,
	        aem_service_port: 4502,
	        aem_service_raw_cmd_stop: sudo systemctl stop aem-author,
	        aem_service_raw_cmd_start: sudo systemctl start aem-author }

## License

Apache 2.0
