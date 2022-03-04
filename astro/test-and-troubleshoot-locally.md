---
sidebar_label: 'Test and Troubleshoot Locally'
title: 'Test and Troubleshoot Locally'
id: test-and-troubleshoot-locally
description: A guide to running an Astro project locally and diagnosing common problems.
---

## Overview

As you develop data pipelines on Astro, we strongly recommend running and testing your DAGs locally before deploying your project to a Deployment on Astro. This document provides information about testing and troubleshooting DAGs in a local Apache Airflow environment with the Astro CLI.

## Run a Project Locally

Whenever you want to test your code, the first step is always to start a local Airflow environment. To run your project in a local Airflow environment, follow the steps in [Build and Run a Project](develop-project.md#build-and-run-a-project-locally).

## View Airflow Task Logs

You can view logs for individual tasks in the Airflow UI. This is useful if you want to troubleshoot why a specific task instance failed or retried.

To access these logs:

1. Access the Airflow UI in your local Airflow environment by going to `http://localhost:8080`.
2. Open the DAG you want to troubleshoot:

    <div class="text--center">
    <img src="/img/docs/open-dag.png" alt="Access DAG from Airflow UI" />
    </div>

3. In the **Tree View**, click on the task run you want to see logs for:

    <div class="text--center">
    <img src="/img/docs/tree-view.png" alt="Task runs in the tree view" />
    </div>

4. Click **Instance Details**:

    <div class="text--center">
    <img src="/img/docs/instance-details.png" alt="Instance details button in the task log menu" />
    </div>

5. Click **Log**:

    <div class="text--center">
    <img src="/img/docs/task-log.png" alt="Log button from a task instance" />
    </div>

## Access Airflow Component Logs

To show logs for your Airflow Scheduler, Webserver, or Metadata DB locally, run the following command:

```sh
astrocloud dev logs
```

Once you run this command, the most recent logs for these components appear in your terminal window.

By default, running `astrocloud dev logs` shows logs for all Airflow components. If you want to see logs for a specific component, add any of the following flags to your command:

- `--scheduler`
- `--webserver`
- `--postgres`

To continue monitoring logs, run `astrocloud dev logs --follow`. The `--follow` flag ensures that the latest logs continue to appear in your terminal window.

## Run Airflow CLI Commands

To run [Apache Airflow CLI](https://airflow.apache.org/docs/apache-airflow/stable/cli-and-env-variables-ref.html) commands locally, run the following:

```sh
astrocloud dev run <airflow-cli-command>
```

For example, the Apache Airflow command for viewing your entire configuration is `airflow config list`. To run this command with the Astro CLI, you would run `astrocloud dev run config list` instead.

## Test DAGs Locally with pytest

To enhance the testing experience for data pipelines, Astro enables users to run DAG unit tests with [pytest](https://docs.pytest.org/en/7.0.x/index.html#), a testing framework for Python. These tests can be run locally against all DAGs in your Astro project and are optimized to catch code-level errors that may not be easily detectable otherwise.

By default, all Astro projects include a `tests` directory in which you can store custom pytests. To start, all projects include one DAG integrity test called `test_dag_integrity.py` that was built by Astronomer. This test checks that:

- All Airflow tasks have required arguments.
- DAG IDs are unique across the Astro project.
- DAGs have no cycles.
- There are no general import or syntax errors.

To run this default test and any others in your `tests` directory, run the following Astro CLI command:

```sh
astrocloud dev pytest
```

For more information about this command, see the [CLI Command Reference](cli-reference/astrocloud-dev-pytest.md).

## Hard Reset Your Local Environment

In most cases, [restarting your local project](develop-project.md#restart-your-local-environment) is sufficient for testing and making changes to your project. However, it is sometimes necessary to kill your Docker containers and metadata DB for testing purposes. To do so, run the following command:

```sh
astrocloud dev kill
```

This command forces your running containers to stop and deletes all data associated with your local Postgres metadata database, including Airflow Connections, logs, and task history.