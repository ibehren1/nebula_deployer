# nebula_deployer
Ansible playbook for the deployment of Nebula overlay network to Linux hosts.

This playbook automates the deployment of a Nebula network.  For more info on Nebula see the GitHub repo of the project: [https://github.com/slackhq/nebula](https://github.com/slackhq/nebula).

## Getting Started
1. Change into the nebula files directory.

    `cd nebula_files`

2. Download the Nebula binary for your platform (this the platform from which you will run the playbook i.e. MacOS/Windows) and untar it so that nebula_files has the `nebula` and `nebula-cert` binaries.

3. Execute the nebula-cert binary to create the root of trust for your Nebula network.

    `./nebula-cert ca -name "Myorganization, Inc"`

    Your nebula_files directory will now contain the `ca.key` and `ca.crt` files.  As mentioned in the Nebula documentation, the `ca.key` file is the most sensitive file you'll create, because it is the key used to sign the certificates for individual nebula nodes/hosts. Please store this file somewhere safe, preferably with strong encryption.

4. Now run commands such as the following to create certificates for your initial hosts.  For deployment with the playbook, use the FQDN of the host as the name, this is what is used by the playbook when copying files to the host.

    ```
    ./nebula-cert sign -name "lighthouse.example.com" -ip "192.168.100.1/24"  
    ./nebula-cert sign -name "client1.example.com" -ip "192.168.100.2/24" -groups "laptop,home,ssh"  
    ./nebula-cert sign -name "client2.example.com" -ip "192.168.100.9/24" -groups "servers"  
    ```

    Your nebula_files directory should now contain a `<hostname>.crt` and `<hostname>.key` for each of the hosts that you ran commands for above.

5. Update the `config.yml` static_host_map section with your hostnames and IPs.

6. Update the Ansible `hosts` file in the root of the repo with the hostnames.

7. Finally, you are ready to run the playbook to deploy, configure and start Nebula on each of your hosts.  From the root of the repo, run the following command to run the playbook.

    `ansible-playbook -u <user> -b -i hosts --private-key </path/to/ssh.pem> main.yml`

    This assumes that you have SSH access setup for a user across all your hosts.

8. Login to your lighthouse node and run some pings to validate communication between your nodes via Nebula.