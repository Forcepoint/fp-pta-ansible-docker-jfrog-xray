# docker-jfrog-xray

For information about PTA and how to use it with this Ansible role please visit https://github.com/Forcepoint/fp-pta-overview/blob/master/README.md

The Jfrog team provides numerous means to install Xray. The method being used here is
docker compose. In particular, the Jfrog team provides a helper script that is meant
to remove the difficulties of working with a complex product, but this makes it slightly
challenging to work with in a Configuration Management fashion as the script is meant
to be run interactively. It can be run without intervention, but you need to figure out what
variables to set ahead of time, which is what has been done here.

This sets up everything needed on a single system via docker-compose with postgres as the database.
It does not allow the setup of a postgres database on another system.

Refer to the Jfrog Guides [Xray and Artifactory One to One Pairing](https://www.jfrog.com/confluence/display/JFROG/Xray+and+Artifactory+One+to+One+Pairing),
[System Requirements](https://www.jfrog.com/confluence/display/JFROG/System+Requirements?utm_source=platform&utm_content=installer#SystemRequirements-Xray-FileHandleAllocationLimit),
[Xray Release Notes](https://www.jfrog.com/confluence/display/JFROG/Xray+Release+Notes), and
[Upgrading Xray](https://www.jfrog.com/confluence/display/JFROG/Upgrading+Xray).

## Requirements

Run the role docker-host on the host.

## Role Variables

### REQUIRED

* docker_jfrog_xray_version: The overall version of Jfrog Xray to setup with docker-compose.
* docker_jfrog_xray_data_dir: The path on the docker host to store all the data which should be persistent.
* docker_jfrog_xray_postgres_password: The password for the Postgres user. This should be vaulted.
* docker_jfrog_xray_shared_jfrog_url: The URL address of the JFrog Platform Instance to connect with. 
  You can copy the JFrog URL from Admin > Security > Settings.
* docker_jfrog_xray_shared_security_joinkey: The secret key used to establish trust between services in the JFrog Platform.
  You can copy the Join Key from Admin > Security > Settings.

### OPTIONAL

* docker_jfrog_xray_postgresql_migrate: When yes, the database is extracted, a new container 
  spun up in its place, and the data reimported. Without examining the version of
  postgres specified in the docker-compose, it's hard to know when this should be run.
  Rule of thumb is to have this run whenever there is a major version change. 
  See https://www.postgresql.org/docs/current/upgrading.html
* docker_jfrog_xray_certs_to_trust: A list of certificates to trust and whether they are remote or not.
  Useful if you're using a private CA for Artifactory's web certificate.

Make sure you get any passwords vaulted so they're not in plain text!

## Dependencies

None

## Example Playbook

Make sure you get that password and joinkey vaulted so they're not in plain text!

    - hosts: server
      vars:
        docker_jfrog_xray_download_host: https://artifactory.company.com/artifactory/releases.jfrog.io/
        docker_jfrog_xray_version: '3.15.3'
        docker_jfrog_xray_data_dir: /home/{{ ansible_user }}/data
        docker_jfrog_xray_postgres_password: ppassword
        docker_jfrog_xray_shared_jfrog_url: https://artifactory.COMPANY.com
        docker_jfrog_xray_shared_security_joinkey: asdf1234
        docker_jfrog_xray_certs_to_trust:
          # You baked your private CA certificate into the base image, use remote_src yes.
          - { path: '/etc/pki/ca-trust/custom/private_ca.pem', remote_src: yes }
          # The RHEL CND certificate. It's not included in the Java truststore by default.
          # May make more sense to get it from the playbook than to bake it into the base image. Use remote_src no.
          - { path: 'files/rhel_cnd.pem', remote_src: no }
      roles:
         - role: docker-jfrog-xray

## License

BSD-3-Clause

## Author Information

Jeremy Cornett <jeremy.cornett@forcepoint.com>
