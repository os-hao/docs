---
sidebar_label: 'Airflow Alerts'
title: 'Configure Airflow Email Alerts on Astronomer'
id: 'airflow-alerts'
---

## Overview

You can utilize Airflow's alerting framework to monitor the health of individual tasks and DAGs across your Airflow Deployments. On Astronomer, setting up alerts requires configuring an SMTP service for delivering each alert.

If your team isn't already using an SMTP service, we recommend one of the following:

- [SendGrid](https://sendgrid.com/)
- [Amazon SES](https://aws.amazon.com/ses/)

This guide provides steps for integrating these external SMTP services on Astronomer Cloud, though any external SMTP service can be used. For more information on adding alerts to your DAG code, read our guide on [Error Notifications in Airflow](https://www.astronomer.io/guides/error-notifications-in-airflow/).

## Integrate SendGrid

[SendGrid](https://sendgrid.com/) is an email delivery service that's easy to set up for Airflow alerts. A free SendGrid account grants users 40,000 free emails within the first 30 days of an account opening and 100 emails per day after that. This should be more than enough emails for most alerting use cases.

To get started with SendGrid:

1. [Create a SendGrid account](https://signup.sendgrid.com). Be prepared to disclose some standard information about yourself and your organization.

2. [Verify a Single Sender Identity](https://sendgrid.com/docs/ui/sending-email/sender-verification/). Because you're sending emails only for internal administrative purposes, a single sender identity is sufficient for integrating with Astronomer. The email address you verify here is used as the sender for your Airflow alert emails.

3. Create a key using SendGrid's web API. In SendGrid, go to **Email API** > **Integration Guide**. Follow the steps to generate a new API key using SendGrid's Web API and cURL.

4. Skip the step for exporting your API Key to your development environment. Instead, execute the generated curl code directly in your command line, making sure to replace `$SENDGRID_API_KEY` in the `--header` field with your copied key.

5. Verify your integration in SendGrid to confirm that the key was activated. If you get an error indicating that SendGrid can't find the test email, try rerunning the cURL code in your terminal before retrying the verification.

6. In the Astronomer UI, open your Deployment and [set the following environment variables](environment-variables):

   ```
   AIRFLOW__SMTP__SMTP_HOST=smtp.sendgrid.net
   AIRFLOW__SMTP__SMTP_STARTTLS=True
   AIRFLOW__SMTP__SMTP_SSL=False
   AIRFLOW__SMTP__SMTP_USER=apikey
   AIRFLOW__SMTP__SMTP_PASSWORD=<your-api-key>   
   AIRFLOW__SMTP__SMTP_PORT=587
   AIRFLOW__SMTP__SMTP_MAIL_FROM=<your-sendgrid-email>
   ```

   >**Note:** To prevent unauthorized users in your Workspace from seeing sensitive information, we recommend storing your `SMTP_PASSWORD` environment variable using an external secrets service.

7. Click **Update Variables** to push your configuration to your Deployment.

## Integrate Amazon SES

This setup requires an AWS account and use of the [AWS Management Console](https://aws.amazon.com/console/).

1. In the AWS Management Console, go to **AWS Console** > **Simple Email Service** > **Email Addresses** to add and verify the email addresses you want to receive alerts.

2. Open the inbox of each email address you specified and verify them through the emails sent by Amazon.

3. In the AWS Console, go to **Simple Email Service** > **SMTP Settings** and use the **Create My SMTP Credentials** button to generate a username and password. This will look similar to an access and secret access key. Write down this username and password for step 5, as well as the **Server Name** and **Port**.

   > **Note:** You won't be able to access these values again, so consider storing them in a password manager.

4. Choose an Amazon EC2 region to use, then write down the code of this server for the next step. Refer to [Amazon's list of available regions and servers](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#concepts-regions) to determine which server best fits your needs.

5. In the Astronomer UI, open your Deployment and [set the following environment variables](environment-variables):

   ```
   AIRFLOW__SMTP__SMTP_HOST=<your-smtp-host>
   AIRFLOW__SMTP__SMTP_PORT=587
   AIRFLOW__SMTP__SMTP_STARTTLS=True
   AIRFLOW__SMTP__SMTP_SSL=False
   AIRFLOW__SMTP__SMTP_USER=<your-aws-username>
   AIRFLOW__SMTP__SMTP_PASSWORD=<your-aws-password>
   AIRFLOW__SMTP__SMTP_MAIL_FROM=<your-email-address>
   ```

   >**Note:** To prevent unauthorized users in your Workspace from seeing sensitive information, we recommend storing your `SMTP_PASSWORD` environment variable using an external secrets service.

6. Click **Update Variables** to push your configuration to your Deployment.