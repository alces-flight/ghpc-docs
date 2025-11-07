# Add a User

## Regular User

A regular user is a non-root user who can access a cluster as themselves and run their workloads.

- Login to domain
- Create user (this will prompt for a temporary password that you will provide to the user alongside their username)
    ```bash
    ipa user-add newuser1 --first New --last User --password
    ```

The user can now access any nodes that are `login` or `compute` types of all clusters. The first time they login successfully they will be prompted to set a new password.

## Site Admin

A site admin user is a privileged user who can utilise sudo to manage and modify a cluster.

- Login to domain
- Create user
```
ipa user-add USERNAME --first USER --last NAME --random
ipa group-add-member SiteAdmins --users siteadmin
ipa user-mod siteadmin --random 
```

The user can now access any nodes of all types (`controller`, `login` and `compute`) of all clusters. The first time they login successfully they will be prompted to set a new password.
