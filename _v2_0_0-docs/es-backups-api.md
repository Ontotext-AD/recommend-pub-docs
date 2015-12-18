---
layout: default
title: ElasticSearch backups API
prev_section: user-actions
category: HowTo's
permalink: v2_0_0-docs/es-backups-api/
---

<div class="info-badge">All example request URLs in this section are relative to the application root. For instance, if the web application is deployed at http://10.0.0.10:8080/backup-es and the example says to request <code>/backups</code>, the full URL would be http://10.0.0.10:8080/backup-es/backups.</div>

The backups API provides the opportunity to create backups of ES indices and restore the state captured by the backups. For this purpose you can either use a scheduler to perform the backups creation in a specific time or you can manually manage the backups yourself.

## Managing the backups manually

### Creating a backup

Each backup is described by its name, its current state and the time its creation started.
To manually create a backup of the current state, use one of the the following calls:

`POST /backups` or 
`POST /backups?backupName=name` 
 
With the first call the backup name depends on the current date-time (for example "2015-12-17t11:08:11.727z"). The second one expects a name containing lowercase letters. An example of a response is:

<pre><code>
{
  "version": "Ontotext Backups API",
  "type": "BACKUP_INFO",
  "status": "OK",
  "response": {
    "name": "2015-12-17t11:08:11.727z",
    "state": "SUCCESS",
    "startDateTime": "2015-12-17T11:08:11.735Z"
  }
}
</code></pre>

It contains the name of the created backup, the current state, and the time when it was started.

### Retrieving a list of the backups

To get s list of all existing backups, use:

`
GET /backups
` 
You will receive a response similar to the following:

<pre><code>
{
  "version": "Ontotext Backups API",
  "type": "BACKUP_INFO",
  "status": "OK",
  "backups": [
    {
      "name": "2015-12-17t11:08:11.727z",
      "state": "SUCCESS",
      "startDateTime": "2015-12-17T11:08:11.735Z"
    },
    {
      "name": "2015-12-17t11:14:32.116z",
      "state": "SUCCESS",
      "startDateTime": "2015-12-17T11:14:32.123Z"
    }
  ]
}
</code></pre>

### Deleting a backup

To delete a specific backup (for example a backup with name "backup_to_delete", use the following request:

`
DELETE /backups/backup_to_delete
` 

The name of the backup that you want to be removed is specified in the path. If the removal is successful you will receive the following response:

<pre><code>
{
  "version": "Ontotext Backups API",
  "type": "SUCCESS",
  "status": "OK",
  "message": "Deletes a backup with name: backup_to_delete"
}

</code></pre>

### Restoring from a backup

To restore the state from a backup, use:

`
POST /backups/{backupName}/restore
`

An example of a successful restoration response is:

<pre><code>
{
  "version": "Ontotext Backups API",
  "type": "SUCCESS",
  "status": "OK",
  "message": "Restored backup with name: 2015-12-17t11:08:11.727z"
}
</code></pre>

## Using a scheduler for managing backups
 
### Retrieving the scheduler default configuration

The scheduler is responsible for:
 
* managing the time for backups creations;
* keeping a certain number of backups; 
* the indices which we want to be backed up. 

The scheduler can be enabled and disabled.
Get the default scheduler configurations with the following request:

`
GET /backups/scheduler/default
`

The response looks like this:

<pre><code>
{
  "version": "Ontotext Backups API",
  "type": "SCHEDULER_INFO",
  "status": "OK",
  "response": {
    "backupsLimit": 5,
    "cronExpression": "0 0 3,15 * * *",
    "enabled": false
  }
}
</code></pre>

By default the scheduler is disabled, the `cronExpression` is set to create backups at 3AM and 3PM (when enabled), the limit of the backups is set to 5. The `indices` field contains a list of the indices to backup. If it is empty or isn't set at all, all indices are included in the backup.

## Resetting the default scheduler configurations

To reset the default scheduler configurations, make the request:

`
POST /backups/scheduler/default
`

It deletes all the changes done to the configurations and restores their default values.

## Changing the scheduler configuration

To change the scheduler configuration, make the request:

`
POST /backups/scheduler
`

<pre><code>
Content-type: application/json

{
    "backupsLimit": 4,
    "cronExpression": "0 0 5 * * *",
    "enabled":true,
    "indices": [
        "index-name-1",
        "index-name-2"
    ]
}
</code></pre>

The request above enables the scheduler to create backups for indices with names `"index-name-1"` and `"index-name-2"` at `4` o'clock in the morning and sets the scheduler to keep the four newest backups. 
In the JSON object you can include an arbitrary subset of the scheduler properties. For example, to enable only the scheduler, make the request:

`POST /backups/scheduler`

<pre><code>
Content-type: application/json
{
    "enabled":true
}
</code></pre>

## Retrieving the current scheduler configuration

Get the current scheduler configuration with the following call:

`
GET /backups/scheduler
`
An example of a response is:

<pre><code>
{
  "version": "Ontotext Backups API",
  "type": "SCHEDULER_INFO",
  "status": "OK",
  "response": {
    "backupsLimit": 4,
    "cronExpression": "0 0 5 * * *",
    "enabled": true
  }
}
</code></pre>

## Scheduler's parameters valid values

* The maximum `backupsLimit` is currently set to 10. If you try to change the limit to a larger value (for example to 20) you will receive the following error response:

<pre><code>
{
  "version": "Ontotext Backups API",
  "type": "ERROR",
  "status": "FAILED",
  "message": "Requested URI: /backup-es/backups/scheduler; Error message: The maximum limit is 10 and the minimum is 1, but 20 was provided"
}
</code></pre>

* The `cronExpression` should contain 6 fields respectively for seconds, minutes, hours, days, months and weekdays ([Spring cron expression format](https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/scheduling/support/CronSequenceGenerator.html)).
* The `enabled` parameter can be set to `true` or `false`.
* The `indices` parameter should be set to a list containing the names of the indices that you want to backup. If the list contains an invalid index name the following response is produced:

<pre><code>
{
  "version": "Ontotext Backups API",
  "type": "ERROR",
  "status": "FAILED",
  "message": "Requested URI: /backup-es/backups/scheduler; Error message: Failed to configure scheduler. Index with name some-invalid-name doesn't exist"
}
</code></pre>
