# docker_update_containers

This role updates or redeploys Docker containers based on their `docker-compose.yml` definitions.

## Purpose

This role allows for updating / recreating containers with modified configurations or newer images.  It simplifies the redeployment process and ensures containers are running the latest desired versions.

This can be used to deploy a single docker containers manually instead of using the `docker_deploy_containers` role.

## Tasks Performed

1.  Validates input for the `container_names` variable.
2.  Finds all `docker-compose.yml` files for the host.
3.  Filters the compose files based on `container_names`.
4.  Fetches environment variables from Vault (if defined).
5.  Removes the existing container stack. ( Does not remove the volumes )
6.  Deploys or redeploys the specified containers using `docker-compose`.

## Variables

*   **`container_names`** (*Required*):  A list of containers to be updated.  Define it in the playbook where you are using this role.

## Important Notes

*   Requires the `community.docker` and `community.hashi_vault` collections.
*   This role removes the existing container stack before redeploying. Ensure you have proper backups or persistent volumes configured if you need to preserve data.
