---
title: "Upgrade to Astro CLI v1.0+"
sidebar_label: "Upgrade to Astro CLI v1.0+"
id: upgrade-astro-cli
description: A list of all breaking changes and upgrade steps related to the major release of Astro CLI v1.0+
---

## Overview

Astro CLI v1.0 delivers several new features to Astronomer Software and establishes a shared CLI framework between Astro and Software users.

As part of this release, several commands and their flags have been updated, resulting in breaking changes for Software users. Use this document to learn about these breaking changes and prepare for your upgrade to Astro CLI v1.0.

## Upgrade Checklist

Before installing Astro CLI v1.0+, complete all of the following steps:

- Review [Breaking Changes](upgrade-astro-cli.md#breaking-changes) in this document.
- Update any CI/CD pipelines or other automated processes that use Astro CLI commands to ensure that these commands do not break After upgrade.
- Review any custom shortcuts in your local CLI terminal to ensure that your shortcuts do not run any deprecated or broken CLI commands.

After you complete these steps, upgrade to Astro CLI v1.0+ by following the steps in [Install the CLI](install-cli.md).

## Breaking Changes

This topic contains all information related to breaking changes included in Astro CLI vXX

Note that this topic does not include information about new non-breaking features and changes. For a summary of these changes, see the [CLI Release Notes](cli-release-notes.md).

### `astro dev init`: New `--use-astronomer-certified` Flag Required for All New Projects Deployed to Software

When you create a new Software project via `astro dev init`, you must now specify the `--use-astronomer-certified` flag if you want to [deploy the project](deploy-code.md) to a Software deployment. This flag initializes your project with the latest version of Astronomer Certified.

If you don't specify this flag, the project will be generated with an Astronomer Runtime image. You can run a project with a Runtime image locally, but you cannot deploy it to your Software installation.

```sh
# Before upgrade
astro dev init
# After upgrade
astro dev init --use-astronomer-certified
```

### `astro auth login/logout` Is Now `astro login/logout`

```sh
# Before upgrade
astro auth login <domain>
astro auth logout
# After upgrade
astro login <domain>
astro logout
```

### `astro workspace create/update` Now Takes All Properties as Flags

When you specify properties for a Workspace using `astro workspace create/update`, you must now specify those properties with new flags.

```sh
# Before upgrade
astro workspace create label=my-workspace
# After upgrade
astro workspace create --label=my-workspace
```

### `astro workspace create/update`: `--desc` Flag Has Been Renamed to `--description`

The flag for specifying a description for a Workspace has been renamed from `--desc` to `--description`.

```sh
# Before upgrade
astro workspace create label=my-workspace --desc="my description"
# After upgrade
astro workspace create --label=my-workspace --description="my description"
```

### `astro workspace user add`: User Email Is Now Specified as a Flag

You must now specify a new user's email using the `--email` flag when running `astro workspace `

```sh
# Before upgrade
astro workspace user add email-to-add@astronomer.io --role WORKSPACE_VIEWER
# After upgrade
astro workspace user add --email email-to-add@astronomer.io --role WORKSPACE_VIEWER
```

### `astro deploy` Now Accepts a Deployment ID Instead of a Release Name

If you want to deploy code to a specific Deployment without having to choose from a list, you must now specify that Deployment's ID instead of its release name when running `astro deploy`.

```sh
# Before upgrade
astro deploy <release-name>
# After upgrade
astro deploy <deployment-id>
```

### `astro workspace sa get` Is Now `astro workspace sa list`

```sh
# Before upgrade
astro workspace sa get
# After upgrade
astro workspace sa list
```

### `astro deployment/workspace sa create`: `--role` Only Accepts Full Role Names

The `--role` flag now accepts either `WORKSPACE_EDITOR`, `WORKSPACE_ADMIN`, or `WORKSPACE_VIEWER`.

```sh
# Before upgrade
astro workspace sa create --role viewer
# After upgrade
astro workspace sa create --role WORKSPACE_VIEWER
```

### `astro deployment create/update` Now Takes All Properties as Flags

You must now specify a Deployment's properties using flags when you run `astro deployment create/update`.

```sh
# Before upgrade
astro deployment create my-deployment --executor=local
# After upgrade
astro deployment create --label=my-deployment --executor=local
```

### `astro deployment user list` Now Takes Deployment ID as a Flag

To see users from a specific Deployment, you must now use the `--deployment-id` flag when running `astro deployment user list`.

```sh
# Before upgrade
astro deployment user list <deployment-id>
# After upgrade
astro deployment user list --deployment-id=<deployment-id>
```

### `astro deployment user add`: `--email` Flag Is Now Required

You must now specify a user's email using the `--email` flag when running `astro deployment user add`.

```sh
# Before upgrade
astro deployment user add <email> --deployment-id=<deployment-id>
# After upgrade
astro deployment user add --email=<email> --deployment-id=<deployment-id>
```

### `astro deployment user delete` Is Now `astro deployment user remove`

```sh
# Before upgrade
astro deployment user delete <email> --deployment-id=<deployment-id>
# After upgrade
astro deployment user remove <email> --deployment-id=<deployment-id>
```

### `astro logs <component>` Is Now Deprecated

The `astro logs` commands has been removed. This command has been non-functional for the last several CLI releases. Instead, use `astro deployment logs <component>` to see logs for a specific Deployment.

### `astro cluster list/switch` Is Now `astro context list/switch`

```sh
# Before upgrade
astro cluster list
# After upgrade
astro context list
```

### `astro deployment sa create`: `--system-sa` and `--user-id` are Deprecated

The `--system-sa` and `--user-id` flags have been deprecated for `astro deployment sa create`. These flags have not been functional for the last several CLI versions.


### `astro dev init`: Shortcut for `--ariflow-version` Is Now `-a`

The shortcut for specifying an Astronomer Certified version for `astro dev init` has been changed from `-v` to `-a`.

```sh
# Before upgrade
astro dev init -v=2.3.0
# After upgrade
astro dev init -a=2.3.0
```