# awssso

## About

_awssso_ Python tool to get short-term credential tokens for CLI/Boto3 operations whit AWS SSO. Allows easy swapping between roles/accounts.

## Motivation

We use several accounts/roles and we need to switch between accounts/roles, grab temporary session credentials and make sure they're the ones used.

This script is a quick work around to quickly switch between accounts/roles.

## How it works

Uses AWS CLI configuration files to give you an interactive command line interface to switch between the profile you want to use. It will then trigger a SSO login session with [aws-vault](https://github.com/99designs/aws-vault) tool and generate a temporary session token for the selected profile to be secure stored. Finally you can export the profile to the environment variables to use it.

## Prerequisites

- [python3](https://www.python.org/downloads/) and [pip](https://docs.python.org/3/installing/index.html)(already installed with python3)

- [aws cli v2](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)

- [aws-vault](https://github.com/99designs/aws-vault) for SSO.

- Dependencies in `requirements.txt` file. Install these with:

```bash
  pip3 install -r requirements.txt
```

## Setting up

- [python3](https://www.python.org/downloads/) and [pip](https://docs.python.org/3/installing/index.html)(already installed with python3)

- Install [aws cli v2](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)

- Install [aws-vault](https://github.com/99designs/aws-vault) to store the SSO login session.

- [Configure your profiles](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-sso.html) to use [aws-vault](https://github.com/99designs/aws-vault). For example:

```ini
[profile sso-prod]
sso_start_url=https://xxxxx.awsapps.com/start
sso_region=us-east-1
sso_account_id=798987340964
sso_role_name=PowerUserAccess

[profile prod]
region=us-east-1
output=json
cli_pager=
credential_process=aws-vault exec sso-prod --json

[profile sso-prod-r]
sso_start_url=https://xxxxxx.awsapps.com/start
sso_region=us-east-1
sso_account_id=798987340964
sso_role_name=ReadOnlyAccess

[profile prod-r]
region=us-east-1
output=json
cli_pager=
credential_process=aws-vault exec sso-prod-r --json
```

- Make the `awssso` executable and copy it somewhere in your `%PATH%`.

- Install the dependencies with `pip3 install -r requirements.txt`

You should be good to go :wink:.

## Usage

You can run `awssso`.

```bash
  awssso
```

Select the profile from a list (_the ones whitouth sso in the name_):

```bash
$ awssso

[?] Please select an AWS config profile: prod
 > prod
   prod-r
   sso-prod
   sso-prod-r
```

Once the profile is selected, the script will create valid SSO credentials that will be stored in aws-vault and you can set the profile in your environment.

```bash
 export AWS_PROFILE=prod
```

## Example

Here is a simple example of the use:

```bash
$ awssso
[?] Please select an AWS config profile: prod
 > prod
   prod-r
   sso-prod-eks
   sso-prod-r

Using profile: prod
{
    "UserId": "AROAS7KQ2YRTYEQQAHJK6:Qbandev@users.noreply.github.com",
    "Account": "798987340964",
    "Arn": "arn:aws:sts::798987340964:assumed-role/PowerUserAccess/Qbandev@users.noreply.github.com"
}

$ export AWS_PROFILE=prod
```

## Inspiration

This script is inspired by Neil 'Jed' Jedrzejewski' [aws-sso-credentials](https://github.com/NeilJed/aws-sso-credentials) repository.
