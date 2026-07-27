# ansible-redis-sentinel

Ansible Playbook to deploy a High Availability Redis with Sentinel

## Getting started

1. Create 3 servers and configure public-key authentication for a regular SSH
   user that has `sudo` permission. Direct root SSH access is not required.
2. Edit `vars.yml` and replace `<YOUR_PASSWORD>` with a secure password.
3. Edit `inventory` and replace `<SERVER_01_IP>`, `<SERVER_02_IP>`,
   `<SERVER_03_IP>`, and `<SSH_USER>` with the corresponding values.
4. Verify that Ansible can connect as the configured user:

    ```bash
    ansible redis -i inventory -m ping
    ```

5. Run playbook `setup-redis-sentinel.yml`. The playbook connects as
   `<SSH_USER>` and uses `sudo` only for privileged tasks:

    ```bash
    ansible-playbook setup-redis-sentinel.yml -i inventory
    ```

   If the SSH user must enter a password for `sudo`, add
   `--ask-become-pass` (or `-K`):

    ```bash
    ansible-playbook setup-redis-sentinel.yml -i inventory --ask-become-pass
    ```

## Notice

1. The configuration for Redis and Sentinel are defined in `redis.conf.j2` and `sentinel.conf.j2`. The playbook explicitly uses `:` to separate each line and use `regexp` to update the corresponding file on the server. Therefore, the playbook will only touch the necessary setting and doesn't modify any other default settings.
2. Make sure that the `role` in `inventory` is correct before running the playbook. For example, if the Sentinel changes the master to a replica. You need to change the `role` to match the current master-replica setup. Otherwise, the playbook will mess up the setting of Sentinel.
