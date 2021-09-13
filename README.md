# SAFE Testnet action

This action provides the following functionality:

- Deploying a testnet by deploying the [SAFE Network](https://github.com/maidsafe/safe_network) node binary to individual Digital Ocean droplets.
- Destroying a previously deployed testnet.

# Usage

This action can be used by including it as a step in a workflow.

```
- name: Launch testnet
  uses: lionel1704/safe-testnet-action@master
  with:
      do-token: ${{ secrets.DO_TOKEN }}
      aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
      aws-access-key-secret: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
      ssh-secret-key: ${{ secrets.SSH_SECRET_KEY  }}
      node-count: 50
      node-version: 0.27.0
```

We can also use a binary built during the workflow by passing the path to the `node-path` input.

# Inputs

|Input|Description|Required|Default|
|---|---|:---:|:---:|
|`do-token`|Digital Ocean Access Token|`true`|-|
|`aws-access-key-id`|AWS Access Key ID|`true`|-|
|`aws-access-key-secret`|AWS Access Key Secret|`true`|-|
|`aws-default-region`|AWS Default region|`false`|`eu-west-2`|
|`ssh-secret-key`|SSH key used to run the nodes on the Digital Ocean droplets|`true`|-|
|`node-count`|Number of nodes to be deployed|`false`|`50`|
|`node-path`|Path to the node binary|`false`*||
|`node-version`|Node version|`false`*||
|`action`|Task to be carried out. Accepts `create` or `destroy`|`false`|`create`|

`*` - Either `node-path` or `node-version` should be provided. If both are supplied `node-version` takes precedence.

# License

This Safe Network repository is licensed under the General Public License (GPL), version 3 ([LICENSE](LICENSE) http://www.gnu.org/licenses/gpl-3.0.en.html).

# Contributing

Want to contribute? Great :tada:

There are many ways to give back to the project, whether it be writing new code, fixing bugs, or just reporting errors. All forms of contributions are encouraged!

For instructions on how to contribute, see our [Guide to contributing](https://github.com/maidsafe/QA/blob/master/CONTRIBUTING.md).