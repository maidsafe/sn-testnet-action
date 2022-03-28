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
|`build-node`|Should the node binary be built? Accepts `true` or `false`|`false`*|`false`|
|`action`|Task to be carried out. Accepts `create` or `destroy`|`false`|`create`|


`*` - Either `node-path` or `node-version` should be provided or `build-node` should be set to `true`. <br>
If both `node-path` and `node-version` are supplied `node-version` takes precedence. `build-node` overrides both of them.

# License

This SAFE Network library is dual-licensed under the Modified BSD ([LICENSE-BSD](LICENSE-BSD) https://opensource.org/licenses/BSD-3-Clause) or the MIT license ([LICENSE-MIT](LICENSE-MIT) https://opensource.org/licenses/MIT) at your option.

# Contributing

Want to contribute? Great :tada:

There are many ways to give back to the project, whether it be writing new code, fixing bugs, or just reporting errors. All forms of contributions are encouraged!

For instructions on how to contribute, see our [Guide to contributing](https://github.com/maidsafe/QA/blob/master/CONTRIBUTING.md).
