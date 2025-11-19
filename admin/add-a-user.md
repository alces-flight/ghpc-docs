# Add a User

## Regular User

A regular user is a non-root user who can access a cluster as themselves and run their workloads.

- Login to domain
- Create user (this will output a temporary password that you will provide to the user alongside their username)
    ```bash
    ipa user-add newuser1 --first New --last User --random --noprivate
    ```
- Add the user to the relevant cluster group(s). Replace `mycluster` with the cluster name.
    ```bash
    ipa group-add-member myclusterusers --users newuser1
    ```
- Switch to the user and setup their Flight SSH key
    ```bash
    su - newuser1
    flight start
    ```

The user can now access any nodes that are `login` or `compute` types of the allowed clusters. The first time they login successfully they will be prompted to set a new password.

**Note: New users will need to login at least once via CLI in order to set their password before they can use the web interface**

## Cluster Admin

A cluster admin user is a privileged user who can utilise sudo to manage a cluster.

- Login to domain
- Create user (this will output a temporary password that you will provide to the user alongside their username)
    ```bash
    ipa user-add newadmin1 --first New --last Admin --random --noprivate
    ```
- Add the user to the relevant cluster group(s). Replace `mycluster` with the cluster name.
    ```bash
    ipa group-add-member myclusterusers --users newadmin1
    ipa group-add-member myclusteradmins --users newadmin1
    ```
- Switch to the user and setup their Flight SSH key
    ```bash
    su - newadmin1
    flight
    ```

The user can now access any nodes of all types (`controller`, `login` and `compute`) of the allowed clusters. The first time they login successfully they will be prompted to set a new password.
