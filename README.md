# docker-jfrog-xray

For information about PTA and how to use it with this Ansible role please visit https://github.com/Forcepoint/fp-pta-overview/blob/master/README.md

Refer to the Jfrog Guides [Xray and Artifactory One to One Pairing](https://www.jfrog.com/confluence/display/JFROG/Xray+and+Artifactory+One+to+One+Pairing),
[System Requirements](https://www.jfrog.com/confluence/display/JFROG/System+Requirements?utm_source=platform&utm_content=installer#SystemRequirements-Xray-FileHandleAllocationLimit),
[Xray Release Notes](https://www.jfrog.com/confluence/display/JFROG/Xray+Release+Notes), and
[Upgrading Xray](https://www.jfrog.com/confluence/display/JFROG/Upgrading+Xray).

## Requirements

Run the role docker-host on the host.

## Role Variables

### REQUIRED

* docker_jfrog_xray_postgres_password: The password for the Postgres user. This should be vaulted.
* docker_jfrog_xray_data: The path on the docker host to store all the data which should be persistent.

### OPTIONAL

* docker_jfrog_xray_tag_jfrog_xray: The tag of the Jfrog Xray container and other related Xray containers to pull.
* docker_jfrog_xray_tag_jfrog_router: The tag of the Jfrog Xray Router container to pull.
* docker_jfrog_xray_tag_jfrog_rabbitmq: The tag of the Jfrog rabbitmq container to pull.
* docker_jfrog_xray_tag_jfrog_postgres: The tag of the Jfrog PostgreSQL container to pull.
* docker_jfrog_xray_postgresql_migrate: When yes, the database is extracted, a new container 
  spun up in its place, and the data reimported. This is run by default when the postgresql 
  tag is changed. You shouldn't need to set this unless you must force the replacement of 
  the postgres data. Afterwards, you should set this back to no.

Make sure you get any passwords vaulted so they're not in plain text!

## Dependencies

None

## Example Playbook

Again, make sure you get that password vaulted so it's not in plain text!

    - hosts: server
      vars:
        docker_jfrog_xray_data: /home/{{ ansible_user }}/data
        docker_jfrog_xray_postgres_password: ppassword
      roles:
         - role: docker-jfrog-xray

## License

BSD-3-Clause

## Author Information

Jeremy Cornett <jeremy.cornett@forcepoint.com>
