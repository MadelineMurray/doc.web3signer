---
title: Configure access to signing keys
description: Configure access to BLS12-381 and secp256k1 signing keys.
sidebar_position: 3
---
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Configure access to signing keys

You can configure access to the signing key by:

- [Creating a key configuration file].
- Using the [`eth2` subcommand options](../reference/cli/subcommands.md#eth2) to bulk load consensus
  layer signing keys stored in [Azure Key Vault](#azure-key-vault), [AWS Secrets Manager](#aws-secrets-manager),
  or [keystore files](#keystore-files).
- Using the [`eth1` subcommand options](../reference/cli/subcommands.md#eth1) to bulk load execution
  layer signing keys stored in [Azure Key Vault](#azure-key-vault) or [keystore files](#keystore-files).

:::note
Bulk loading is only available when using keys stored in Azure Key Vault, AWS Secrets Manager,
or keystore files, and can be used in combination with key configuration files.
:::

## Use key configuration files

For each signing key, define the parameters to access the key in a [key configuration file].
You can create a separate configuration file for each key, or specify multiple configurations in a
single file by adding a triple-dash separator (`---`) between configurations.

The configuration file must be YAML-formatted, and can use any naming format, but must have the `.yaml` extension.

Place one or more key configuration files in a single directory which you specify when starting Web3Signer.
Use the [`--key-store-path`](../reference/cli/options.md#key-store-path) option to specify the
location of the key configuration files.

```bash
web3signer --key-store-path=/Users/me/keyFiles/ eth2
```

## Bulk load keys

### Azure Key Vault

You can bulk load keys that are stored in Azure Key Vault using the Web3Signer
[`eth1` subcommand options](../reference/cli/subcommands.md#eth1) or
[`eth2` subcommand options](../reference/cli/subcommands.md#eth2).

<Tabs>

  <TabItem value="Consensus layer client" label="Consensus layer client" default>

```bash
web3signer eth2 --azure-vault-enabled=true --azure-client-id=87efaa5b-4029-4b54-98bb2e2e8a11 \
--azure-client-secret=0DgK4V_YA99RPk7.f_1op0-em_a46wSe.Z \
--azure-tenant-id=34255fb0-379b-4a1a-bd47-d211ab86df81 \
--azure-vault-name=AzureKeyVault
```

  </TabItem>
  <TabItem value="Execution layer client" label="Execution layer client" >

```bash
web3signer eth1 --azure-vault-enabled=true --azure-client-id=87efaa5b-4029-4b54-98bb2e2e8a11 \
--azure-client-secret=0DgK4V_YA99RPk7.f_1op0-em_a46wSe.Z \
--azure-tenant-id=34255fb0-379b-4a1a-bd47-d211ab86df81 \
--azure-vault-name=AzureKeyVault
```

  </TabItem>
</Tabs>

### AWS Secrets Manager

You can bulk load consensus layer keys that are stored in AWS Secrets Manager using the Web3Signer
[`eth2` subcommand options](../reference/cli/subcommands.md#eth2).

The AWS bulk load mode supports loading multiple consensus layer keys from the same secret, if keys
are stored with a line terminating character such as `\n`.
This saves cost when dealing with a large number of keys.
Up to 200 keys can be stored under a secret name.

```bash
web3signer eth2 --aws-secrets-enabled=true --aws-secrets-access-key-id=AKIA...EXAMPLE \
--aws-secrets-secret-access-key=sk...EXAMPLE \
--aws-secrets-region=us-east-2
```

### Keystore files

You can bulk load consensus layer or execution layer keys that are stored as keystore files using the Web3Signer
[`eth1` subcommand options](../reference/cli/subcommands.md#eth1) or
[`eth2` subcommand options](../reference/cli/subcommands.md#eth2).

<Tabs>

  <TabItem value="Consensus layer client" label="Consensus layer client" default>

```bash
web3signer eth2 --keystores-path=/Users/me/keystores \
--keystores-passwords-path=/Users/me/passwds
```

  </TabItem>
  <TabItem value="Execution layer client" label="Execution layer client" >

```bash
web3signer eth1 --keystores-path=/Users/me/keystores \
--keystores-passwords-path=/Users/me/passwds
```

  </TabItem>
</Tabs>

Use the `eth1` or `eth2` `--keystores-password-file` or `--keystores-passwords-path` command line option to specify
keystore passwords.

<!-- Link -->

[key configuration file]: ../reference/key-config-file-params.md
[Creating a key configuration file]: #use-key-configuration-files