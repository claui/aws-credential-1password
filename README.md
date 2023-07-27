# aws-credential-1password

This is an unofficial [1Password](https://1password.com/) **credentials helper** for the [AWS CLI](https://aws.amazon.com/cli/).

## Overview

This tool enables the `aws` command to **use AWS credentials from your 1Password vault.**

That way, your AWS credentials stay encrypted at rest on your device, which is known to be a good security practice. You no longer have to keep your credentials in plain text in your `~/.aws/config` file.

The tool is neither affiliated with nor endorsed by 1Password or AWS. It’s also **alpha-quality and largely untested.** Use it at your own risk.


## Prerequisites

This tool requires Linux or macOS. You also need an active 1Password membership.


## Installation

### Manual installation

To install `aws-credential-1password` manually:

1. Install the [1Password command-line tool](https://1password.com/downloads/command-line/).

2. Install [jq](https://stedolan.github.io/jq/).

3. Copy the `aws-credential-1password` executable into a directory that is in your `PATH`.


### Installing via the Arch User Repository (AUR)

To install `aws-credential-1password` via the [AUR](https://wiki.archlinux.org/title/Arch_User_Repository), use your favorite AUR helper to install the [aws-credential-1password](https://aur.archlinux.org/packages/aws-credential-1password/) package.

For example, if you use [aurutils](https://github.com/AladW/aurutils), run `aur sync aws-credential-1password` and then `sudo pacman -Syu aws-credential-1password`.


### Installing via Homebrew

To install `aws-credential-1password` via [Homebrew](https://brew.sh/), run:

```
brew install claui/public/aws-credential-1password
```

If you put it in your `~/.aws/config` file, the `aws` command will get secrets from your 1Password vault.


## Configuration

1. Open your terminal.

2. Confirm that your 1Password CLI is properly configured and signed into your 1Password vault. To do that, check the output of the following shell command:

    ```
    $ op vault list
    ```

3. Next, open the 1Password app and create a login item with two fields named `Access Key ID` and `Secret Access Key`. Fill in your AWS credentials into those fields.

4. Open the 1Password preferences and go to the _Advanced_ tab. Tick the checkbox(es) that enable UUID and JSON copying. The checkbox may be called _Show debugging tools_ but the name can vary depending on which version of the 1Password app you have.

5. Obtain the UUID of your login item in 1Password. To do that, right-click on the login item and choose either UUID or JSON, depending on what the app offers you. If you choose JSON, paste the result to a text editor and locate the UUID manually. Copy the UUID to your clipboard.

6. Create a file `config` in your `~/.aws` directory if it’s not already there.

7. Edit your `~/.aws/config` as follows:

    ```
    [default]
    credential_process = /path/to/aws-credential-1password OP_VAULT OP_ITEM ACCESS_KEY_ID_FIELDNAME SECRET_ACCESS_KEY_FIELDNAME
    ```

8. In the config file, replace the fragment `/path/to/aws-credential-1password` with the actual path to your `aws-credential-1password` script.

9. Run `op vault list` to see the UUIDs of your vaults. In the config file, replace the fragment `OP_VAULT` with the UUID of your vault.

10. In the config file, replace the fragment `OP_ITEM` with the UUID of your login item.

11. In the 1Password app (or the exported JSON), look at the **names** of the 1Password fields that contain your AWS access key ID and your secret access key. In the config file, replace the fragments `ACCESS_KEY_ID_FIELDNAME` and `SECRET_ACCESS_KEY_FIELDNAME` with those field names.

12. To confirm that everything is working, run:

    ```
    $ aws iam get-user
    ```


## Usage

Sign into the 1Password CLI, then use the `aws` command normally.


## License

Copyright (c) 2021 The `aws-credential-1password` authors

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
