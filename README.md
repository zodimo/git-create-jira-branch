<p align="center">
  <img
    width="256px"
    title="git-create-jira-branch Logo"
    alt="git-create-jira-branch Logo"
    src="./assets/logo.webp"
  >
</p>

# git-create-jira-branch - Setup feature branches for your Jira tickets with one command.

Creates feature branches based on your Jira tickets type and description.

```bash
$ git create-jira-branch MYAPP-1234
> Successfully created branch: 'feat/MYAPP-1234-sluggified-description-used-as-branchname'
```

## Usage

Since this command starts with `git-` all commands can be run via
`git-create-jira-branch` or as a git subcommand with `git create-jira-branch`.

Due to a limitation in the awesome [cli
library](https://github.com/Effect-TS/cli) used, all options must be passed
before the jira ticket key argument.

### Create a new branch from your current `HEAD`

Using the default JIRA_KEY_PREFIX

```bash
git-create-jira-branch 1324
```

Or fully specified:

```
git-create-jira-branch MYAPP-1234
```

### Create a new branch based on some other revision

To create a new branch based on your `master` branch:

```bash
git-create-jira-branch -b master MYAPP-1234
```

### Reset an already existing branch

Pass the `-r|--reset` flag to reset an already existing branch to the current
`HEAD` or the specified base revision (with `-b`)

```bash
git-create-jira-branch -r MYAPP-1234
```

### Open tickets in your browser

1. For the current branch:
   ```bash
   $ git create-jira-branch --open
   > Opening ticket url 'https://gcjb.atlassian.net/browse/GCJB-164' in your default browser...
   ```
2. For a given ticket:
   ```bash
   $ git create-jira-branch --open GCJB-1234
   > Opening ticket url 'https://gcjb.atlassian.net/browse/GCJB-1234' in your default browser...
   ```

### `wizard` mode

Use the `--wizard` option to enter `wizard` mode. This will prompt you for the
Jira ticket key and additional options and build the appropriate command line
for you.

```bash
git-create-jira-branch --wizard
```

## Setup

### Install

The cli can be installed from `npm`. It assumes you have git installed on your
system and the `git` command to be available on your `$PATH`.

```bash
npm i -g git-create-jira-branch
```

### Configuration

#### For Jira Cloud

1. Create a Jira API Token [See Jira
   Docs](https://support.atlassian.com/atlassian-account/docs/manage-api-tokens-for-your-atlassian-account/)
1. Add the created API Token, your login email, the base url of your Jira
   instance and optionally a default Jira key prefix to your environment.
   For example in your `.bashrc` or `.zshrc`:
   ```bash
   export JIRA_USER_EMAIL="YOUR_JIRA_LOGIN_EMAIL"
   export JIRA_API_TOKEN="YOUR_API_TOKEN"
   export JIRA_API_URL="https://jira.mycompany.com"
   export JIRA_KEY_PREFIX="MYAPP"
   ```

#### For Jira Data Center

1. Create a Jira PAT (Personal Access Token) [See Jira
   Docs](https://confluence.atlassian.com/enterprise/using-personal-access-tokens-1026032365.html)
1. Add the created Jira PAT, the base url of your Jira instance and optionally a
   default Jira key prefix to your environment.
   For example in your `.bashrc` or `.zshrc`:
   ```bash
   export JIRA_PAT="YOUR_PERSONAL_ACCESS_TOKEN"
   export JIRA_API_URL="https://jira.mycompany.com"
   export JIRA_KEY_PREFIX="MYAPP"
   ```

### Setup shell completions

The cli can generate shell completion scripts for `bash`,`zsh` and `fish`. To
generate and print the script for your shell run:

```bash
git-create-jira-branch --completions (bash|zsh|fish)
```

To install the completions for your shell, run the above command and pipe the
output to a file and source it in your shell config.

E.g. for `bash`:

```bash
git-create-jira-branch --completions bash > ~/.git-create-jira-branch-bash-completions
echo "source \$HOME/.git-create-jira-branch-bash-completions" >> ~/.bashrc
source ~/.bashrc
```

## Technologies used

This project was started as an excuse to explore the
[Effect](https://effect.website/) ecosystem and was written with only
`@effect/*` packages as it's runtime dependencies. It uses:

| Package                                                        | Usage                                                                                                                             |
| -------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| [effect](https://github.com/Effect-TS/effect)                  | The core effect system and runtime.                                                                                               |
| [@effect/cli](https://github.com/Effect-TS/cli)                | Command line handling and option parsing.<br>The `wizard` mode and `--completions` option are automatically provided by this lib. |
| [@effect/platform](https://github.com/Effect-TS/platform)      | For its http client.                                                                                                              |
| [@effect/platform-node](https://github.com/Effect-TS/platform) | For its shell command executor.                                                                                                   |

It uses the Jira API to fetch the details for a ticket and calls out directly to `git` for branch creation.

Tests were written using [`vitest`](https://vitest.dev). The testsuite can be run using `npm run test`.
