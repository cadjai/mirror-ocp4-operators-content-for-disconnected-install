Role Name
=========

A utility role to mirror OpenShift Container Platform 4.6+ (OCP4) content for disconnected install.
It uses the opm tool along with the built in ocp command (mirror, image...) to bundle and decompress contents. 

Requirements
------------


Role Variables
--------------

For the operator bundle tasks:  
- ansible_name_module: The name of the module this role is part of. This is used to track context when this role is run in a playbook as part of other plays.  
- bundle_file_name: Name of the xz format compressed artifact bundle file (e.g. operators-bundle.tar.xz)  
- dir_bundle_location: Location of the bundle on the host the role/playbook is run against.  
- default_operator_registry_username: Username to use to authenticate to the Source Operator registry.  
- default_operator_registry_password: Password to use to authenticate to the Source Operator registry.  
- default_operator_registry: Source Registry from which operators are mirrored (e.g. registry.redhat.io)  
- registry_container_name: The name of the container registry to use to mirror operators related contents (e.g. mirror-registry).  
- registry_container_dir: The name of the directory structure on the controller host that will be mounted into the registry container to save the container image layers and blobs. 
- registry_container_image: The name of the registry container to run (e.g. docker.io/library/registry:2).  
- operator_registries_to_mirror: The various operators that will be mirrored (e.g.redhat-operators, community-operators, redhat-certified-operators...). This is a dictionary with keys like source, container_port, and mirrored_operartor_list where :  
  - source is the FQDN of the registry and repository and version of the registry index being used to mirror the operator content (e.g. registry.redhat.io/redhat/redhat-operator-index:v4.6)
  - container_port is the name of the port published by the container (e.g. 50051).
  - host_port is the name of the port on the controller to bind the container to (e.g. 50051). It has to be different for each operator index container.
  - mirrored_operator_list is the list of operators to be mirrored if pruning operators to provided comma separated list (e.g. '3scale-operator,apicast-operator').
- opm_client_binary: The path to the opm client binary to be used to mirror operators if not already installed on the controller.  
- grpcurl_binary: The path to the grpcurl client binary to be used to prune operators if pruning a subset of operators.  
- install_grpcurl: The boolean used to toggle the grpcurl binary install if not already install on host.  
- install_opm: The boolean used to toggle the opm binary install if not already install on host.  

For the operator unbundle tasks( reusing some of the variable from the images unbundle above to keep minimal list of variables):  
- dir_bundle_staging: Name of the staging diretory the compressed bundle will be uncompressed in and used for the container registry if needed.  
- bundle_file_location: Fully qualified path to the bundle file to uncompress.  
- registry_admin_username: Username to use to authenticate to the destination registry.  
- registry_admin_password: Password to use to authenticate to the destination registry.  
- registry_host_fqdn: FQDN of the destination registry.  
- registry_host_port: Port of the destination registry.  
- source_operators_repository: The source registry for operator to mirror (e.g. registry.redhat.io)  
- source_operator_index: The operator index image name (e.g. redhat-operator-index)  
- source_operator_index_tag: The operator index tag (e.g. v4.6)  
- local_olm_repository: The destination repository to story the operator content on the destination registry (e.g. openshift4/olm-mirror)  

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------
To create the operator bundle use the sample play below:

    - hosts: localhost
      tasks:
         - name: Create operator bundle
           import_role:
             name: mirror-operator
             tasks_from: mirror-operators.yml
             
To push the operator bundle content into the destination registry use the sample play below:

    - hosts: localhost
      tasks:
         - name: Push operator bundle content into the destination registry
           import_role:
             name: mirror-operator
             tasks_from: push-operators.yml
             

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
