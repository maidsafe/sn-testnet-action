# SAFE Network Testnet action

This action provides the following functionality:

- Deploying a testnet by deploying the [SAFE Network](https://github.com/maidsafe/safe_network) node binary to individual Digital Ocean droplets.
- Destroying a previously deployed testnet.

# Usage

This action can be used by including it as a step in a workflow.

```
- name: Launch testnet
  uses: maidsafe/sn_testnet_action@master
  with:
      do-token: ${{ secrets.DO_TOKEN }}
      aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
      aws-access-key-secret: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
      ssh-secret-key: ${{ secrets.SSH_SECRET_KEY  }}
      node-count: 50
      node-version: 0.27.0
```

We can also use a binary built during the workflow by passing the path to the `node-path` input.

You can ensure your testnet is always cleaned up in a workflow run by adding a job like this:
```
kill-if-fail:
  name: kill testnet on fail
  runs-on: ubuntu-latest
  if: |
    always() &&
    (needs.launch-testnet.result=='failure' ||
     needs.client.result=='failure' ||
     needs.api.result=='failure' ||
     needs.cli.result=='failure')
  needs: [launch-testnet, client, api, cli]
  steps:
    - name: Kill testnet
      uses: maidsafe/sn_testnet_action@master
      with:
        do-token: ${{ secrets.DO_TOKEN }}
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-access-key-secret: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        action: 'destroy'
    - name: Upload event file
      uses: actions/upload-artifact@v2
      with:
        name: event-file
        path: ${{ github.event_path }}
```

Obviously, you need to substitute the job names here with your own.

# Inputs

| Name                      | Description                                                                                                            | Required | Default       |
|---------------------------|------------------------------------------------------------------------------------------------------------------------|----------|---------------|
| action                    | Accepts 'create', 'destroy', 'resources', 'logs', 'list-inventory', 'print-inventory'.                                 | true     |               |
| ansible-vault-password    | Password for Ansible vault.                                                                                            | true     |               |
| ansible-verbose           | Set to use verbose output for Ansible                                                                                  | true     | false         |
| aws-access-key-id         | AWS access key ID                                                                                                      |          |               |
| aws-access-key-secret     | AWS access key                                                                                                         |          |               |
| aws-region                | AWS region                                                                                                             |          | eu-west-2     |
| beta-encryption-key       | Supply an encryption key that will be used with the auditor.                                                           |          |               |
| bootstrap-node-vm-count   | Number of bootstrap node VMs to be deployed                                                                            |          |               |
| do-token                  | Digital Ocean Authorization token                                                                                      | true     |               |
| faucet-version            | Supply a version for the faucet. Otherwise the latest will be used.                                                    |          |               |
| log-format                | Set the log format for the nodes, can be 'json' or 'default'. Defaults to 'default' if not set.                        |          | default       |
| network-contacts-file-name| Provide a name for the network contacts file                                                                           |          |               |
| node-count                | Number of nodes service instances to be started                                                                        |          |               |
| node-vm-count             | Number of node VMs to be deployed                                                                                      |          |               |
| public-rpc                | Set to make node manager RPC daemons publicly accessible                                                               | true     | false         |
| protocol-version          | The network protocol version can be set to 'restricted' or any custom version string.                                  |          |               |
| provider                  | The cloud provider. Accepts 'aws' or 'digital-ocean'.                                                                  | true     |               |
| re-attempts               | The number of times to re-run testnet-deploy in case of failures.                                                      |          | "0"           |
| rust-log                  | Set RUST_LOG to this value for testnet-deploy                                                                          | false    |               |
| safenode-features         | Comma-separated list of features to be used when building a branch of safenode                                         |          |               |
| safenode-version          | Supply a version for safenode. Otherwise the latest will be used.                                                      |          |               |
| safenode-manager-version  | Supply a version for safenode-manager. Otherwise the latest will be used.                                              |          |               |
| safe-network-branch       | Build binaries from the specified safe_network branch. The testnet will use these binaries.                            |          |               |
| safe-network-user         | Build binaries from the safe_network repository owned by this org or username. The testnet will use these binaries.    |          |               |
| ssh-secret-key            | SSH key used to run the nodes on the Digital Ocean droplets                                                            |          |               |
| subnet-id                 | If running on AWS, this is the subnet of the VPC on which the VMs will be created.                                     |          |               |
| security-group-id         | If running on AWS, this is the ID of the security groups the VMs will use.                                             |          |               |
| testnet-name              | The name of the testnet.                                                                                               | true     |               |
| testnet-deploy-branch     | The branch for the sn-testnet-deploy repository. Enables using forks to test changes for testnet-deploy.               |          | main          |
| testnet-deploy-user       | The user or organisation for the sn-testnet-deploy repository. Enables using forks to test changes for testnet-deploy. |          | maidsafe      |
| uploader-vm-count         | Number of uploader VMs to be deployed                                                                                  |          |               |

# License

This SAFE Network library is dual-licensed under the Modified BSD ([LICENSE-BSD](LICENSE-BSD) https://opensource.org/licenses/BSD-3-Clause) or the MIT license ([LICENSE-MIT](LICENSE-MIT) https://opensource.org/licenses/MIT) at your option.

# Contributing

Want to contribute? Great :tada:

There are many ways to give back to the project, whether it be writing new code, fixing bugs, or just reporting errors. All forms of contributions are encouraged!

For instructions on how to contribute, see our [Guide to contributing](https://github.com/maidsafe/QA/blob/master/CONTRIBUTING.md).
