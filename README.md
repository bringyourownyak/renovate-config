# Renovate Bot configuration and workflow

This repository contains the configuration and the GitHub Actions workflow for running the Whitesource Renovate bot
for proof-of-concept purposes.

# Table of Contents

- [Usage](#usage)
- [Links](#links)

# Usage

## Configuration

The GitHub Actions workflow allows the user to run a self-hosted version of the Renovate Bot which allows for more
flexibility in how the bot behaves. The self-hosted bot requires slightly more configuration to get it running - that
configuration is defined in the `default.json` file, and the workflow that then uses the configuration for running the
bot is defined in `.github/workflow/renovate.yaml`.

The bot is configured to keep itself up to date as well by including this repository in the list of tracked 
repositories. The automatically generated `renovate.json` configuration file contains the boilerplate configuration
that enables the bot to track the GitHub Actions' versions and keep them up to date.

All other repositories' configuration is also defined within those repositories' respective `renovate.json`
configuration files - refer to the `default.json` configuration for a list of repositories, and to their `renovate.json`
files for information on specific dependency tracking settings.

## Workflow

The GitHub Actions workflow consists of two jobs described below.

### `renovate-dry-run`

This job is set up to run on `push` events to any branch.  It performs a 
[dry run](https://docs.renovatebot.com/self-hosted-configuration/#dryrun) of the Renovate bot which only checks for 
validity of configuration, and lists actions that will be taken, but doesn't actually perform any actions on the 
repositories. The main purpose of this job is to be used as the CI part of the CI/CD pipeline. 

This job uses the same configuration as the `renovate` job described below, however it uses `jq` to append the `dryRun`
configuration option to the `default.json` configuration as part of the pipeline so that I wouldn't need to store and
update two essentially similar configuration files.

### `renovate`

This job runs either on a schedule defined in the workflow file, or via manual invocation. Every time the job runs it
scans all configured repositories and creates any necessary branches and PRs for keeping all supported dependencies up 
to date. This job serves as the CD part of the CI/CD pipeline.

# Links

- [Renovate documentation.](https://docs.renovatebot.com)
- [Renovate GitHub Action.](https://github.com/renovatebot/github-action)
