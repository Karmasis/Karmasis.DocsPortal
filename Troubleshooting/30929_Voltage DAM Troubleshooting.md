<img src="media/image1.png" style="width:8.29306in;height:11.69931in" /><img src="media/image2.png" style="width:2.4058in;height:0.82614in"
alt="A close up of a sign Description automatically generated" />

Troubleshooting Guide

**Voltage DAM**

**Revision Records**

<table>
<colgroup>
<col style="width: 6%" />
<col style="width: 18%" />
<col style="width: 22%" />
<col style="width: 20%" />
<col style="width: 15%" />
<col style="width: 16%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>No.</strong></th>
<th><strong>Update Date</strong></th>
<th><strong>Voltage DAM Version</strong></th>
<th><p><strong>Added/Updated</strong></p>
<p><strong>/Deleted</strong></p></th>
<th><strong>Place of Change</strong></th>
<th><strong>Description</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>1</td>
<td>29.09.2023</td>
<td>v1.0.0</td>
<td>Added</td>
<td>General</td>
<td>First Publish</td>
</tr>
</tbody>
</table>

**NOTE**

Copyright © 2022 OpenText. All rights reserved. Trademarks owned by
OpenText. One or more patents may cover this product(s). For more
information, please visit
[www.opentext.com](http://www.opentext.com)/patents

**Table of Contents**

[1. Introduction [1](#introduction)](#introduction)

[2. VDAM Agent High Memory Usage
[1](#vdam-agent-high-memory-usage)](#vdam-agent-high-memory-usage)

[3. VDAM Agent Not Creating Logs
[2](#vdam-agent-not-creating-logs)](#vdam-agent-not-creating-logs)

[4. VDAM Query Parse Problem
[3](#vdam-query-parse-problem)](#vdam-query-parse-problem)

[5. Log Accumulation On MSMQ
[4](#log-accumulation-on-msmq)](#log-accumulation-on-msmq)

[6. Dashboard Login Problem
[5](#dashboard-login-problem)](#dashboard-login-problem)

[7. Elasticsearch Security Warning
[6](#elasticsearch-security-warning)](#elasticsearch-security-warning)

[8. Elasticsearch Service Not Starting
[8](#elasticsearch-service-not-starting)](#elasticsearch-service-not-starting)

[9. Infraskope Service Logs Not Processing
[9](#infraskope-service-logs-not-processing)](#infraskope-service-logs-not-processing)

[10. DSIM Not Working [11](#dsim-not-working)](#dsim-not-working)

[11. DSTAP Not Working [12](#dstap-not-working)](#dstap-not-working)

[12. VDAM Agent High Cpu Usage
[13](#vdam-agent-high-cpu-usage)](#vdam-agent-high-cpu-usage)

[13. Collector-Agent Connection Issue
[14](#collector-agent-connection-issue)](#collector-agent-connection-issue)

[14. DSIM Agent Upgrade Failure
[15](#dsim-agent-upgrade-failure)](#dsim-agent-upgrade-failure)

[15. DSTAP Agent Upgrade Failure
[16](#dstap-agent-upgrade-failure)](#dstap-agent-upgrade-failure)

[16. Elasticsearch Health Status Red
[17](#elasticsearch-health-status-red)](#elasticsearch-health-status-red)

[17. Eventviewer Error Event_Id: 9449
[18](#eventviewer-error-event_id-9449)](#eventviewer-error-event_id-9449)

[18. Total Number Of Shards Per Node Has Been Reached
[19](#total-number-of-shards-per-node-has-been-reached)](#total-number-of-shards-per-node-has-been-reached)

[19. MSSQL 1827 Problem [19](#mssol-1827-problem)](#mssol-1827-problem)

[20. Archive And Backup Failure
[20](#archive-and-backup-failure)](#archive-and-backup-failure)

[21. Elasticsearch Low Disk Watermark Error
[21](#elasticsearch-low-disk-watermark-error)](#elasticsearch-low-disk-watermark-error)

[22. Auditdb Suspect Mode
[22](#auditdb-suspect-mode)](#auditdb-suspect-mode)

[23. Log Bloating In Collector Path
[23](#log-bloating-in-collector-path)](#log-bloating-in-collector-path)

[24. Ip-Related Connection Issue Symptom:
[24](#ip-related-connection-issue-symptom)](#ip-related-connection-issue-symptom)

[25. Hostname-Related Connection Issue Symptom:
[25](#hostname-related-connection-issue-symptom)](#hostname-related-connection-issue-symptom)

[26. Infraskope Cannot Connect To MSMQ Symptom:
[26](#infraskope-cannot-connect-to-msmq-symptom)](#infraskope-cannot-connect-to-msmq-symptom)

[27. Antivirus Exclusion List
[27](#antivirus-exclusion-list)](#antivirus-exclusion-list)

[28. MSMQ Service Unaccessible
[28](#msmq-service-unaccessible)](#msmq-service-unaccessible)

# 1. Introduction

Problems that may be encountered during the usage of Voltage DAM are
explained in this document with their symptoms, causes and solutions.

# 2. VDAM Agent High Memory Usage

**Symptom:**

1.  A warning has occurred, indicating that the agent is using high
    memory during the monitoring process.

2.  When checking the details of the relevant agent on the VDAM
    Dashboard, high memory usage of the agent is observed in the
    real-time diagnostics panel.

3.  There is a slowdown on the database server where the agent is
    installed.

**Cause:**

1.  For Windows: The **MaxDSAMemory** String in Regedit may not have
    been set with restrictions.

2.  For Linux: If this situation applies to the Linux agent, the
    parallel parser setting may be incorrect.

3.  Traffic between databases may be monitored. These interfaces should
    be removed from the DSTAP settings.

4.  Max and min memory values may not have limited the agent's
    configuration through the dashboard.

5.  In a database server that generates many logs, the agent's memory
    amount may have been increased for performance.

**Solution:**

1.  If number 1 matches the Cause, a megabyte restriction value should
    be set in Regedit on the relevant database server, corresponding to
    the System Memory amount. (If you want to allocate 6 gigabytes of
    memory, calculate it as 6x1024).

2.  If number 2 matches the Cause, the parallel parser value should be
    increased in multiples of 2.

3.  If number 3 matches the Cause, these interfaces should be removed
    from the DSTAP settings.

4.  If number 4 matches the Cause, observe the system memory amount of
    the database server where the agent is installed, then right-click
    on the agent showing high memory usage on the dashboard after
    monitoring average use for a short time, and go to Edit Setting -\>
    DSTAP -\> adjust the **mem.limit.max** and **mem.limit.min** values.

5.  If number 5 matches the Cause, there may be a large number of
    queries sent to the database server by other servers. To handle
    unnecessary logs or similarly repeated queries, open the Agents
    screen through the dashboard, then go to Policies -\> New Policy -\>
    enter Database and Policy Name. For example, a policy could be
    written as **drop\|field_name\|Drop_desired_component**.

# 3. VDAM Agent Not Creating Logs

**Symptom:**

1.  There is not any log flow from the relevant machine where the agent
    is installed.

2.  When checking the relevant directory on the DB server (for Linux:
    /var/spool/dataskope, for Windows:
    C:\ProgramData\Karmasis\Dataskope\MsgStorage), it is found to be
    empty.

3.  Predefined alarms were triggering in the Dashboard.

**Cause:**

1.  On the Windows side, the "baykus" session may not have been created
    under Management -\> Extended Events -\> Sessions because the "ALTER
    ANY EVENT SESSION" permission was not granted to the "NT/AUTHORITY"
    user on the DB server where the agent is installed.

2.  The /var/spool/dataskope folder may not have been created due to
    improper installation of the agent. If log flow is secured with SSL,
    logs may not be written because they are encrypted.

3.  If there is a failover (active-passive) structure, the relevant
    server may be in a passive node at that moment.

4.  The relevant database may be using different ports besides the
    default port.

5.  Dataskope Agent Services may have stopped.

**Solution:**

1.  If number 1 matches the Cause, close and disable the agent services
    on the server, then uninstall the agent through the Programs and
    Features. Afterward, grant the "ALTER ANY EVENT SESSION" permission
    to the NT/AUTHORITY user. Once the permission is granted, reinstall
    the agent and confirm that the "baykus" session is created.

2.  If number 2 matches the Cause, remove the agent with the "dstool
    cleanup_host" command and reinstall it with a root privileged user.

3.  If number 3 matches the Cause, check the Agent's SSL support. (Refer
    to the table for supported DB types.)

4.  If number 4 matches the Cause, inspect the logs on the active node
    or temporarily switch the passive node to active for observation.

5.  If number 5 matches the Cause, identify the used ports, right-click
    on the relevant agent in the Agents screen, go to Edit Settings -\>
    DSTAP -\> select the used DB type, and enter the relevant ports
    separated by ",."

6.  If number 6 matches the Cause, for Linux, start DSIM and DSTAP
    processes; for Windows, start DSIM and Dataskope SQL Server Agent,
    then check the Message File directory (for Linux:
    /var/spool/dataskope, for Windows:
    C:\ProgramData\Karmasis\Dataskope\MsgStorage).

# 4. VDAM QUERY PARSE PROBLEM 

**Symptom:**

1.  Some fields in incoming logs are missing or have empty values.

**Cause:**

1.  There may be corruption or displacement in the relevant mapping
    files.

2.  Missing or outdated mappings could be the cause.

3.  Parsing feature may be disabled in the Collector.

4.  The parsing feature may be disabled on the Agent.

**Solution:**

1.  If number 1 matches the Cause, report it to the DevOps team.

2.  If number 2 matches the Cause, contact the Support team.

3.  If number 3 matches the Cause, set the
    **ExtractTableColumnsFromQuery** value to true in the
    **settings.yml** file in the folder where the collector is
    installed.

4.  If number 4 matches the Cause, enable the parsing feature for the
    relevant database via DSTAP settings on the dashboard.

<span id="_MSMQ_ÜZERİNDE_LOG" class="anchor"></span>

# 5. LOG ACCUMULATION ON MSMQ 

**Symptom:**

1.  Warning received indicating that the disk with MSMQ installed is
    full.

2.  Alert seen on the Dashboard Home panel indicating log accumulation.

**Cause:**

1.  Accumulation of noise events may have occurred.

2.  The MSMQ disk's read rate is higher than the write rate, and due to
    this sustained increase, the disk has become completely full.

3.  The existing architecture may not be able to handle the incoming log
    volume.

4.  Infraskope Server may be stopped or producing errors.

5.  More than 2000 logs per second may be coming to MSMQ.

6.  The I/O capacity of the MSMQ disk may be insufficient.

**Solution:**

1.  If 1 matches the Cause, for a temporary solution, the ".mq" files on
    the full MSMQ disk should be moved to an empty disk using the "move
    disk_name\msmq\storage\*.mq destination_path" command. For a
    permanent solution, after moving the .mq files, a faster SSD disk
    should be added to the system, and Technical Support should be
    informed to transfer MSMQ.

2.  If 2 matches the Cause, it may be necessary to add an SSD disk to
    increase the read speed of the MSMQ disk.

3.  If 3 matches the Cause, on the server where the Collector is
    located, go to Computer Management -\> Services and Applications -\>
    Message Queuing -\> Private Queues. Then, identify the server with
    log accumulation, and for the agent installed on that server, go to
    Policies -\> New Policy -\> enter Database and Policy Name. For
    example, you can write a policy like:
    **drop\|field_name\|Component_to_be_dropped**.

4.  If 4 matches the Cause, error status should be checked via Event
    Viewer.

5.  If 5 matches the Cause, a new collector should be added to the node.

6.  If 6 matches the Cause, contact the support team.

# 6. DASHBOARD LOGIN PROBLEM

**Symptom:**

1.  An error is encountered when the user logs into the dashboard.

2.  There may be a version mismatch.

**Cause:**

1.  The region-language setting may be incompatible.

2.  There may be a version mismatch.

<img src="media/image4.png" style="width:2.66701in;height:1.62104in"
alt="A screenshot of a computer error message Description automatically generated" />

3.  IIS may be down and unable to connect to the web service. The error
    shown in the image below is likely being received.

<img src="media/image5.png" style="width:2.72257in;height:1.02096in"
alt="A screen shot of a computer Description automatically generated" />

4.  One of the pools may have stopped. The error shown in the image
    below is likely being received.

<img src="media/image6.png" style="width:2.75591in;height:1.0293in"
alt="A screenshot of a computer Description automatically generated" />

**Solution:**

1.  If 1 matches the Cause, correct the server's date, time, and region
    settings.

2.  If 2 matches the Cause, start the Elasticsearch service.

3.  If 3 matches the Cause, start the Internet Information Services
    (IIS).

4.  If 4 matches the Cause, go to Internet Information Services (IIS)
    -\> Server_name Home -\> Application Pools -\> Identify the stopped
    App pool and restart it.

# 7. ELASTICSEARCH SECURITY WARNING

**Symptom:**

1.  An "Elasticsearch Security Warning" appears when logging into the
    dashboard.

**Cause:**

1.  This warning occurs because the necessary settings have not been
    applied with the Elasticsearch Security Tool.

2.  The **xpack** values in the **elasticsearch.yml** file located in
    the C:\elasticsearch-7.16.3\config directory may have been set to
    false.

**Solution:**

1.  If 1 matches the Cause, follow these steps:

- Extract the "Elasticsearch Security Tool" from the ".zip" file located
  in the "7 – Elasticsearch" folder.

- Copy its contents to the "C:\elasticsearch-7.16.3" directory.

- Run "Infraskope.ES.Elastic.Security.exe."

Configuration details:

- Elasticsearch Service Name: Find the Elasticsearch service under
  Services and determine the service name from the Service Name section.

- Elasticsearch URL ([http://localhost:9200](http://localhost:9200/)):
  If security is applied from the master node server, you can press
  Enter.

- SQL Server Name: Enter the hostname of the server.

- SQL Server User Name: Enter the authorized user, which is typically
  "sa."

- SQL Server Password: Enter the sa password.

After pressing Enter, wait for the password to be generated for a single
node. If you have a cluster, navigate to the
"C:\elasticsearch-7.16.3\config" directory. Copy the
"elastic-certificates" and "elasticsearch.keystore" files and paste them
into the same directory on other Elasticsearch nodes. Return to the
"C:\elasticsearch-7.16.3\config" directory of the Master Node, open
elasticsearch.yml, and copy the following lines from the bottom:

> xpack.security.enabled: true
>
> xpack.security.transport.ssl.enabled: true
>
> xpack.security.transport.ssl.verification_mode: certificate
>
> xpack.security.transport.ssl.keystore.path: elastic-certificates.p12
>
> xpack.security.transport.ssl.truststore.path: elastic-certificates.p12

Paste these lines into the elasticsearch.yml file on other nodes,
starting from the bottom. Then, restart the Elasticsearch service on
that node. After completing these steps, return to the Master Node, and
you will see that the tool generates a User Name and Password after some
time.

1.  If 2 matches the Cause, follow these steps:

- Navigate to the C:\elasticsearch-7.16.3\config directory.

- Open the elasticsearch.yml file and, if the xpack lines are present,
  change the values from false to true as follows:

> xpack.security.transport.ssl.enabled: false
>
> xpack.security.transport.ssl.verification_mode: certificate
>
> xpack.security.transport.ssl.keystore.path: elastic-certificates.p12
>
> xpack.security.transport.ssl.truststore.path: elastic-certificates.p12

(If you have a cluster, apply the same process to other Elasticsearch
nodes.)

# **8. ELASTICSEARCH SERVICE NOT** STARTING 

**Symptom:**

1\. Elasticsearch service not working again after it is restarted.

**Cause:**

1.  The Elasticsearch folder may not be created under C:\Users\<current
    user\>\AppData\Local\Temp.

2.  The network path may not be provided for Archive&Backup files, or
    access permissions may not be granted to the relevant files.

3.  There may be errors in the Elasticsearch.yml content.

4.  File access permissions may not be granted.

**Solution:**

1.  If 1 matches the Cause, follow these steps:

    - Go to C:\Users\<current user\>\AppData\Local\Temp\\ and manually
      create the Elasticsearch folder to ensure the "geoip-databases"
      and "jna-xxxx" folders are created.

    - Start the service after creating the Elasticsearch folder.

2.  If 2 matches the Cause, provide a network path for the Archive and
    Backup folders.

3.  If 3 matches the Cause, follow these steps:

    - Go to the C:\elasticsearch-7.16.3\config directory.

    - Open Elasticsearch.yml and confirm the accuracy of the
      cluster.name, path.repo, path.data, discovery.seed_hosts, and
      cluster.initial_master_nodes values.

    - Additionally, check the snapshot status for the relevant database
      through DSTAP settings in the Dashboard.

    - Examine the ClusterName.log file in the
      C:\elasticsearch-7.16.3\logs folder.

4.  If 4 matches the Cause, examine the relevant files in the
    C:\elasticsearch-7.16.3\logs folder.

# 9. INFRASKOPE SERVICE LOGS NOT PROCESSING 

**Symptom:**

1.  When searching for logs through the dashboard, logs do not appear
    within the specified time range.

2.  Accumulation in MSMQ. (See [MSMQ log
    accumulation](#_MSMQ_ÜZERİNDE_LOG))

3.  Infraskope cannot connect to IIS.

4.  "msmq may not be installed: Insufficient resources to perform
    operation" error on the dashboard.

**Cause:**

1.  IIS, Infraskope service, syslog service, or Elasticsearch service
    may have stopped.

2.  It could be blocked due to antivirus software.

3.  MSMQ limit may have been exceeded.

4.  Log formats sent by the firewall may not be appropriate.

5.  MSMQ role may not be installed and/or has been removed.

**Solution:**

1.  If 1 matches the Cause, restart the Infraskope service.
    Additionally, review the Event Viewer logs related to Infraskope
    Server. Check the status of the required services and manually start
    any stopped services.

2.  If 2 matches the Cause, consider that it may be blocked by antivirus
    software. Make necessary exclusions (exclusion) settings for
    antivirus software when required. [ANTIVIRUS EXCLUSION
    LIST](#_ANTİVİRÜS_EXCLUSION_LİSTESİ)

3.  If 3 matches the Cause, you can increase the MSMQ limit or reset it
    as needed.

> <img src="media/image7.png" style="width:5.46341in;height:3.86458in"
> alt="A computer screen shot of a computer Description automatically generated" />

4.  If 4 matches the Cause, modify the data format within the
    "SyslogConfig.xml" file located at "C:\Program Files
    (x86)\Karmasis\Infraskope Syslog Sensor XXXX" path to match the
    appropriate format for logs sent from the firewall.

5.  If 5 matches the Cause, see [MSMQ log
    accumulation](#_MSMQ_ÜZERİNDE_LOG).

# 10. DSIM NOT WORKING 

**Symptom: **

1.  When checking the agent installed on the relevant database server
    via the dashboard, it displays "unknown."

2.  When searching for relevant events on the dashboard's search screen,
    no logs appear.

3.  When attempting to connect to the agent with DSIMTool, a connection
    error occurs.

**Cause:  **

1.  The "dsim" service on the Linux server or "Dataskope Installation
    Manager (SQLDSIM)" service on the Windows server may have stopped.

2.  The one-way opened "8765" port from the collector server to the
    servers where the database is installed may be closed through the
    firewall.

3.  The agent certificate may be corrupted or changed.

4.  Antivirus or similar programs on the database server may be
    preventing the agent from running.

 

**Solution:  **

1.  If 1 matches the Cause, start the service on the Linux server with
    the appropriate commands for the version. For example, on an el7
    version server, start the service with the "systemctl start dsim"
    command using the root user or a user with root privileges. On a
    Windows server, start the service with the "net start SQLDSIM"
    command using an admin user or a user with admin privileges.

2.  If 2 matches the Cause, contact the firewall team and request the
    one-way opening of port 8765. After the port is opened, you can
    check the port status with the command "netstat -ano \| findstr
    "8765"" (Windows) or "netstat -tuln \| grep 8765" (Linux).

3.  If 3 matches the Cause, add a new certificate from the Dashboard -\>
    Agents -\> Options -\> Default Certificate field.

4.  If 4 matches the Cause, add the agent's used directories to the
    exclusions list of the antivirus software. [ANTIVIRUS EXCLUSION
    LIST](#_ANTİVİRÜS_EXCLUSION_LİSTESİ)

# 11. DSTAP NOT WORKING

**Symptom: **

1.  When checking the agent installed on the DB server via the
    dashboard, the "DSTAP STATUS" column shows "not running" for the
    relevant Windows server and "unknown" for the Linux server.

2.  When searching for events on the dashboard's search screen, only
    heartbeat logs are found, and no other logs are present.

**Cause:  **

1.  When checking the agent installed on the DB server via the
    dashboard, the "DSTAP STATUS" column shows "not running" for the
    relevant Windows server and "unknown" for the Linux server.

2.  When searching for events on the dashboard's search screen, only
    heartbeat logs are found, and no other logs are present.

**Solution:  **

1.  If 1 matches the Cause, start the service on the Linux server with
    the appropriate commands for the version. For example, on an el7
    version server, start the service with the "systemctl start dsim"
    command using the root user or a user with root privileges. On a
    Windows server, start the service with the "net start DataskopeSql"
    command using an admin user or a user with admin privileges.

2.  If 2 matches the Cause, add the directories used by the agent to the
    exclusions list of the antivirus software. [ANTIVIRUS EXCLUSION
    LIST](#_ANTİVİRÜS_EXCLUSION_LİSTESİ)

# 12. VDAM AGENT HIGH CPU USAGE

**Symptom: **

1.  The System Monitoring team encounters a situation of high CPU usage,
    resulting in the relevant agent being identified as the component
    causing the high usage.

2.  When checking agent details on the Agent screen in the Dashboard,
    the real-time monitoring shows high CPU usage.

3.  There is a slowdown on the DB server where the agent is installed.

**Cause:  **

1.  The System Monitoring team encounters a situation of high CPU usage,
    resulting in the relevant agent being identified as the component
    causing the high usage.

2.  When checking agent details on the Agent screen in the Dashboard,
    the real-time monitoring shows high CPU usage.

3.  There is a slowdown on the DB server where the agent is installed.

**Solution:  **

1.  If 1 matches the Cause, observe the System CPU usage of the DB
    server where the agent is installed. Then, after monitoring the
    average usage for a short period, right-click on the agent that
    shows high CPU usage in the Dashboard and go to Edit Setting -\>
    DSTAP -\> fix the cpu.parallel_parsers value.

2.  If 2 matches the Cause, there may be a large number of queries sent
    by application servers. You can write a policy for unnecessary logs
    or queries that are automatically sent in succession.

3.  If 3 matches the Cause, you can move the message files located in
    the "C:\ProgramData\Karmasis\Dataskope\MsgStorage" directory for
    Windows or "/var/spool/dataskope" for Linux to another directory.
    Then, in the Dashboard, change the directory where the relevant
    agent reads logs from on the Agents screen.

# 13. COLLECTOR-AGENT CONNECTION ISSUE 

**Symptom: **

1.  When checking the agent installed on the DB server in the Dashboard,
    it shows "unknown."

2.  When searching for events from the search screen in the Dashboard,
    no logs are displayed.

3.  When trying to connect to the agent with DSIMTool, a connection
    error occurs.

4.  When adding the agent-installed server to the Dashboard, an error is
    encountered.

>  

**Cause:  **

1.  The "DSIM" service on a Linux server or "DSIM (SQLDSIM)" service on
    a Windows server may have stopped.

2.  The "8765" port, which is unidirectionally opened from the server
    where the Collector is installed to the servers with DB
    installations, may be closed via the firewall.

3.  There may be antivirus or similar programs on the DB server that are
    preventing the agent from running.

4.  The agent may not have been properly installed on the DB servers.

5.  The "Dataskope Collector Service" on the Collector server may have
    stopped.

**Solution:  **

1.  If 1 matches the Cause, the service should be started on the Linux
    server with version-appropriate commands. For example, on an el7
    version server, use the "systemctl start dsim" command as the root
    or a user with root privileges. On a Windows server, use the "net
    start SQLDSIM" command as an administrator or a user with admin
    privileges to start the service.

2.  If 2 matches the Cause, contact the firewall team and request the
    unidirectional opening of port 8765. After opening the port, you can
    check the port's status with commands like "netstat -ano \| findstr
    "8765"" (Windows) or "netstat -tuln \| grep 8765" (Linux).

3.  If 3 matches the Cause, add the directories used by the agent to the
    exclusions list of the antivirus software. [ANTIVIRUS EXCLUSION
    LIST](#_ANTİVİRÜS_EXCLUSION_LİSTESİ)

4.  If 4 matches the Cause, check the status of the service on Linux
    using "systemctl status dsim" (if the server is running el7) or use
    "sc query" to check the "SQLDSIM" service on Windows. If the
    services are not visible, you may need to reinstall the agent.

5.  If 5 matches the Cause, run the Collector server as an administrator
    with the command prompt and then start the "Dataskope Collector
    Service" using the command "net start "Dataskope Collector
    Service"."

# 14. DSIM AGENT UPGRADE FAILURE 

**Symptom: **

1.  When attempting to upgrade the agent to a higher version than the
    current DSIM version via the dashboard, an error is encountered.

**Cause:  **

1.  If the agent version in the dashboard is 4084+, an attempt may have
    been made to upgrade the DSIM version without updating to
    Certificate Version 2.

2.  The wrong signed file may have been selected instead of
    "dsim.signed."

**Solution:  **

1.  If 1 matches the Cause, it's possible that the agent on the DB
    server has become inactive due to this type of update. To view the
    certificate version in the dashboard's Agents screen, right-click in
    the area where the columns are located, then click on "Select
    Columns," and select "Certificate Version" to add it. Right-click on
    the agent associated with the DB server to be updated with DSIM,
    select "Deploy New Certificate." In the "Security Certificate
    (.crt)*" field, add the updated dsim_server security certificate. Go
    to the "Key File (.key)*" field, and add the updated
    dsim_server.key. Click the "Save" button, and after a short while,
    you will see that the version has changed to 2. Right-click on the
    relevant agent in the dashboard, select "Deploy Agent -\> dsim."
    Then, select the dsim.signed file from the updated agent folder, and
    the agent's DSIM will be updated.

<!-- -->

1.  If 2 matches the Cause, make sure that the "dsim.signed" file from
    the installation folder of the current version is selected for the
    update.

## 

# 15. DSTAP AGENT UPGRADE FAILURE 

**Symptom: **

1.  When attempting to upgrade the agent to a higher version than the
    current DSTAP version via the dashboard, an error is encountered.

**Cause:  **

1.  If the agent version in the dashboard is 4084+, an attempt may have
    been made to upgrade the DSTAP version without updating to
    Certificate Version 2.

2.  The wrong signed files may have been selected instead of
    "dstap.signed," "dstool.signed," "libdpsl.so.signed," and
    "libdpslth.so.signed."

**Solution:  **

1.  If 1 matches the Cause, it's possible that the agent has become
    inactive on the DB server because it was updated in this way, and
    therefore, the agent needs to be reinstalled. In the dashboard's
    Agents screen, right-click in the area where the columns are
    located, then click on "Select Columns," and select "Certificate
    Version" to add it. Right-click on the agent associated with the DB
    server to be updated with DSTAP, select "Deploy New Certificate." In
    the "Security Certificate (.crt)*" field, add the updated
    dsim_server security certificate. Go to the "Key File (.key)*"
    field, and add the updated dsim_server.key. Click the "Save" button,
    and after a short while, you will see that the version has changed
    to 2. Then, zip the "dstap.signed," "dstool.signed,"
    "libdpsl.so.signed," and "libdpslth.so.signed" files found in the
    updated agent folder, select "Deploy Agent -\> dstap" in the
    dashboard, choose the zipped file, and update DSTAP.

2.  If 2 matches the Cause, ensure that the "dstap.signed,"
    "dstool.signed," "libdpsl.so.signed," and "libdpslth.so.signed"
    files from the updated agent version are selected correctly.

<span id="_ELASTICSEARCH_HEALTH_STATUS" class="anchor"></span>

# 16. ELASTICSEARCH HEALTH STATUS RED

**Symptoms:**

1.  Event Viewer shows error 9449, and Elasticsearch is marked as "red"
    in the details section.

2.  Accumulation is observed on the MSMQ disk.

**Cause:**

1.  The free space on the Esdata disk may be less than 10%.

2.  Index corruption may have occurred.

3.  There may be a hardware or software issue with the Esdata disk.

4.  If the service has just been restarted, not all indexes may have
    been opened due to high index sizes on the system.

**Solution:**

1.  If 1 is due to Cause 1, you should increase the disk size. Going
    below 10% seems to be a very low threshold, so you can expand the
    disk size as needed. You can change Elasticsearch disk settings
    through Sense as follows (values may vary depending on the
    situation):

>  PUT /\_cluster/settings 
>
> { 
>
>     "persistent" : { 
>
> "cluster.routing.allocation.disk.watermark.flood_stage": "50gb
> "cluster.routing.allocation.disk.watermark.high": "100gb", 
>
> "cluster.routing.allocation.disk.watermark.low": "150gb" 
>
>     } 
>
> } 

2.  If 2 is the cause, you can restore using ESBACKUP.

3.  If 3 is the cause, you should contact the disk provider.

4.  If 4 is the cause, you should wait for the service to open all
    indexes.

# 17. EVENTVIEWER ERROR EVENT_ID: 9449

**Symptoms:  **

1.  Accumulation is occurring in MSMQ.

2.  Current logs are not visible on the dashboard.

 

**Cause:**

1.  Event Viewer may show "AuditDB Offline" in log details.

2.  Event Viewer may display an Elasticsearch Red error in log details.

3.  Event Viewer may indicate a connection error to MSMQ in log details.

4.  Event Viewer may show a "Total number of shards per node has been
    reached" error in log details.

5.  Event Viewer may report an error indicating an excessive number of
    fields in log details.

**Solution:**

1.  If 1 matches the cause, you should truncate the 'elaResourceHistory'
    and 'elaSessionLog' tables using SSMS.

2.  If 2 matches the cause, you should follow the steps for resolving
    [Elasticsearch Red errors](#_ELASTICSEARCH_HEALTH_STATUS).

3.  If 3 matches the cause, you should follow the steps for resolving
    Infraskope [MSMQ connection errors](#_INFRASKOPE_CANNOT_CONNECT).

4.  If 4 matches the cause, you should follow the steps for resolving
    the "[Total number of shards per node has been
    reached](#total-number-of-shards-per-node-has-been-reached)" error.

5.  If 5 matches the cause, you should identify the resource with too
    many fields by running the following commands in Sense:

                              PUT events_2023_08_03/\_settings   
                                   {   
                                        "index": {   
                                                 
 "mapping.total_fields.limit": 4000   
                                                    }   
                                  } 

> OR

 ** **

                              PUT /events_2023_08_03/\_settings   
                                    {   
                                       "index": {   
                                               "refresh_interval":
"1s"   
                                                      }   
                                      } 

Run the commands. Later, you can correct the source with a high number
of fields in the log template.

# 18. TOTAL NUMBER OF SHARDS PER NODE HAS BEEN REACHED

**Symptoms: **

1.  There may be a backlog in MSMQ.

2.  Slowdowns observed on the dashboard.

3.  Log writing may stop.

>  

**Cause:**

1.  The log retention period in the live database may have been set to
    more than 1 year.

2.  The archiving system may be malfunctioning.

**Solution: **

1.  If 1 is applicable, you can adjust the live database settings to
    have a log retention period of 1 year or less, or you can add a new
    node.

2.  If 2 is applicable, you should follow the steps to [resolve
    archiving and backup errors.](#_ARCHIVE_VE_BACKUP)

# 19. MSSOL 1827 PROBLEM

**Symptoms: **

1.  Users see logs containing the word "error" in the search bar on the
    dashboard.

2.  An error with code 1827 is observed under the Application section in
    Event Viewer.

 

**Cause: **

1.  This issue is caused by the increase in the size of the SQLDATA
    folder where login logs are stored.

**Solution: **

1.  In SSMS (SQL Server Management Studio), select the
    'elaResourceHistory' and 'elaSessionLog' tables, and perform a
    "select 1000 rows" operation. Then, in the window that opens, delete
    everything after "from" and replace it with "truncate table."
    Complete the operation. Next, select the "auditDB" section,
    right-click, and choose "shrink" and "database." In the window that
    opens, check the box and execute the script within the same window.
    Finally, close the window with "cancel." From the SSMS page, rerun
    the newly created rule.

>  

<span id="_ARCHIVE_VE_BACKUP" class="anchor"></span>

# 20. ARCHIVE AND BACKUP FAILURE 

**Symptoms: **

1.  The number in the "warmarchive" section on the dashboard may be
    greater than 180.

2.  The colors of the archive and backup disks in the disk structure may
    turn red.

3.  There may be a date deficiency in the "localhost:9200/\_cat/indices"
    section when accessed via the web.

 **Cause: **

1.  The event leading to Symptoms 1 and 2 is the filling of archive and
    backup disks in terms of storage capacity.

2.  The event leading to Symptom 3 is the curator, which is scheduled to
    run every night at 1:00 AM, not running.

3.  The Elasticsearch service and/or IIS being stopped is the event
    causing Symptom 3.

**Solution: **

1.  If cause 1 and 2 are present, consider keeping the archive and
    backup disks on separate drives. As a temporary solution, you can
    increase the storage capacity of the full disks.

2.  If cause 2 is present, manually execute the curator task.

3.  If cause 3 is present, restart the Elasticsearch service and IIS if
    they are stopped.

# 21. ELASTICSEARCH LOW DISK WATERMARK ERROR 

**Symptoms: **

1.  When entering the dashboard, the following error is encountered.

2.  The ESDATA disk on Elasticsearch servers appears in red and is full.

<img src="media/image8.png" style="width:5.58123in;height:2.35417in"
alt="metin, yazı tipi, ekran görüntüsü içeren bir resim Açıklama otomatik olarak oluşturuldu" />

**Cause: **

1.  The Elasticsearch service may have stopped due to the inability to
    perform archive operations.

2.  The Elasticsearch log entries may have stopped due to the disk space
    on Elasticsearch servers falling below 10%.

**Solution: **

1.  If it corresponds to cause 1, see [ARCHIVE AND BACKUP
    FAILURE](#_ARCHIVE_VE_BACKUP). Elasticsearch service and settings
    should be checked and, if necessary, modified and restarted.

2.  If it corresponds to cause 2, as a temporary solution for the full
    "ESDATA," you can prevent Elasticsearch from throwing errors by
    reducing the default "disk watermark" settings. You can execute the
    following commands in Sense within Chrome for the relevant settings:

> PUT \_cluster/settings   
> {   
>     "transient" : {   
>     "cluster.routing.allocation.disk.threshold_enabled": true ,   
>    
> "cluster.routing.allocation.disk.watermark.flood_stage":"100gb",   
>     "cluster.routing.allocation.disk.watermark.high": "150gb",   
>     "cluster.routing.allocation.disk.watermark.low": "200gb"   
>       
>   }   
> } “
>
> Please run this code. Additionally, for the full ESDATA, increase the
> disk size and restart the Elasticsearch service.

#  22. AuditDB SUSPECT MODE

**Symptom:  **

1.  In SQL Management Studio, the text "Recovery" appears next to the
    relevant database.

**Cause: **

1.  The server may have been unnecessarily restarted multiple times.

2.  The server's power may have been suddenly interrupted.

3.  One of the database files (.mdf, .ldf) may have been deleted from
    the disk. 

   

**Solution:   **

1.  If it corresponds to cause 1 and 2, MSSMQ database recovery should
    be initiated.

- EXEC SP_RESETSTATUS 'AuditDB'  

> GO  

- USE Master  

> ALTER DATABASE AuditDB SET SINGLE_USER  
>
> GO  

- USE Master  

> ALTER DATABASE AuditDB SET EMERGENCY  
>
> GO  

- USE Master  

> DBCC CheckDB ('AuditDB',REPAIR_ALLOW_DATA_LOSS)  
>
> GO  

- USE Master  

> ALTER DATABASE AuditDB SET MULTI_USER;  
>
> GO  

- USE Master  

> ALTER DATABASE AuditDB SET ONLINE  
>
> GO 

 

2.  If it corresponds to Symptom 3, database restoration should be
    performed from previous backups.  

# 23. LOG BLOATING IN COLLECTOR PATH 

**Symptom:  **

1.  It has been observed that the path where the Collector is installed
    on the disk has grown excessively.

2.  Continuous reduction in free space is observed on the C: drive
    (system drive).

>  

**Cause: **

1.  This may be due to not changing the Dump Path and LongQueries Path
    settings in Collector settings, leaving them at their default
    settings.

   

**Solution:   **

1.  If it corresponds to cause 1, go to the directory where the
    Collector is installed (C:\Program
    Files\Karmasis\Karmasis.Dataskope.Collector) and reconfigure the
    "DumpsPath" and "LongQueriesPath" settings in the settings.yaml file
    to a path outside the system disk.

# 24. IP-RELATED CONNECTION ISSUE Symptom:

1.  When trying to log in to the Dashboard, a warning is displayed.

2.  Agents on the Dashboard appear as inactive, and Event Viewer does
    not show event ID 9448.

3.  The Elasticsearch service is not running.

**Cause:**

1.  The machine's IP address may have been changed.

2.  An error may be encountered when trying to log in to the Dashboard.

3.  The IP addresses of the agents may have been changed.

4.  The Elasticsearch service is running, but there may be a
    misconfiguration in the Elasticsearch.yml file.

**Solution:**

1.  If it corresponds to cause 1, check the DNS settings if an "A"
    record is set, or correct the "A" record in the Hosts file (enter
    the new IP address).

2.  If it corresponds to cause 2, update the "WEBSERVICEIPADDR" value in
    the registry of the agents from the regedit file located at
    "Computer\HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\EventLogForwarderd"
    to the new IP address.

3.  If it corresponds to cause 3, the Elasticsearch.yml file may need to
    be updated for cluster configuration. Ensuring correct hostname
    resolution for Archive and Backup shares is important.

4.  In the Web.config file located at C:\inetpub\wwwroot\ElfWebService,
    update the value of "WebServiceHostAddr" to the NEW IP ADDRESS.

\<add key="WebServiceHostAddr" value="NEW IP ADDRESS" /\>

# 25. HOSTNAME-RELATED CONNECTION ISSUE Symptom:

1.  When trying to log in to the Dashboard Main Panel, a warning or
    password error is encountered.

2.  The "A" record in DNS or Hostname may not have been updated.

3.  Event ID: 9448 is not visible when checked in the Event Viewer
    service.

4.  The Elasticsearch service is running, and the path.repo: \["\NEW
    HOSTNAME\esrepo\HOTARCHIVE","\\ NEW HOSTNAME\esrepo\WARMARCHIVE","\\
    NEW HOSTNAME\esrepo\BACKUP"\] in the Elasticsearch.yml file may be
    incorrectly written.

**Cause:**

1.  The machine's Hostname may have been changed.

2.  If an "A" record is set with DNS, check the DNS settings, or check
    the "A" record in the Hosts file and correct it with the new
    hostname.

3.  The agents may not have been able to find the Infraskope server.

4.  The Elasticsearch service is running, and there may be an incorrect
    configuration in the Elasticsearch.yml file.

**Solution:**

1.  Edit the web.config file located under
    C:\inetpub\wwwroot\ElfWebService with the new hostname.

2.  If it corresponds to cause 2, check the DNS settings if an "A"
    record is set, or check the "A" record in the Hosts file and update
    the hostname "A" record with the new hostname.

3.  If it corresponds to cause 3, in the registry under
    "Computer\HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\EventLogForwarder,"
    update the HostName value with the new hostname.

4.  If it corresponds to cause 4, correct the path.repo value in the
    Elasticsearch.yml file.

<span id="_INFRASKOPE_CANNOT_CONNECT" class="anchor"></span>

# 26. INFRASKOPE CANNOT CONNECT TO MSMQ Symptom:

1.  MSMQ disk may be corrupted or removed.

2.  Roles under Computer Management\Message Queuing\Private Queues may
    have been deleted.

3.  The service may have been manually stopped.

**Cause:**

1.  In case of Symptom 1, the MSMQ disk might be corrupted or removed.

2.  In case of Symptom 2, roles under Computer Management\Message
    Queuing\Private Queues might have been deleted.

3.  In case of Symptom 3, the service might have been manually stopped.

**Solution:**

1.  If it corresponds to cause 1, check the MSMQ disk and, if necessary,
    re-add it.

2.  If it corresponds to cause 2, check the files under
    M:\msmq\storage\LQS and, if missing, recreate the agents.

3.  If it corresponds to cause 3, check the Message Queuing service.

<span id="_ANTİVİRÜS_EXCLUSION_LİSTESİ" class="anchor"></span>

# 27. ANTIVIRUS EXCLUSION LIST 

**Symptom: **

1.  When checking the status of services that should be active
    (elasticsearch, infraskope server, infraskope syslog, infraskope
    agent health check, dataskope collector service), it is observed
    that despite the services being running, some or all logs cannot be
    found in dashboard searches.

2.  Event ID: 9448 has been received, but it is observed that logs are
    not being written to disk.

3.  It can be observed that some modules of the agent are not working
    (e.g., USB activities are not visible while the screenshot service
    is running).

**Cause: **

1.  This could be due to antivirus programs installed on the server
    interfering with the application's modules.

**Solution: **

1.  If it corresponds to cause 1, you can resolve the issue by adding
    exclusions to the antivirus software.

- Program Files (x86)/karmasis (C:\Program Files (x86)\Karmasis)

- Program Files/karmasis (C:\Program Files\Karmasis)

- elasticsearch (C:\elasticsearch-7.16.X)

- inetpub/wwwroot (C:\inetpub\wwwroot)

- elfagent (C:\ElfAgent)

- esdata (X:\ESDATA)

- esarchive (X:\ESREPO\HOTARCHIVE)

- esbackup (X:\ESREPO\BACKUP)

- sql (X:\INFASKOPEDB)

# 28. MSMQ SERVICE UNACCESSIBLE

**Symptom:**

1.  Infraskope server service is observed to be stopped.

2.  Unable to log in through the dashboard, and an "Internal Server
    Error" is encountered.

3.  In Event Viewer, it is observed that Event ID "9448" entries are not
    appearing under the application section.

**Cause:**

1.  MSMQ role may have been removed or not installed.

**Solution:**

The MSMQ role needs to be installed, and the following steps should be
followed:

1.1. Open Server Manager.

<img src="media/image9.png" style="width:2.90293in;height:3.29184in" />

<img src="media/image10.png" style="width:3.02793in;height:1.71537in"
alt="A screenshot of a computer Description automatically generated" />

1.2. Install the MSMQ role and its sub-roles as shown in the images.

1.3. Run the "preparation2012R2.cmd" script.

<img src="media/image11.png" style="width:2.38207in;height:4.38217in"
alt="A screenshot of a computer Description automatically generated" />

1.4. Under "Private Queues," add the following queues: ela, elaalerts,
elacmd, elasms, elax, and elasyslog.

<img src="media/image12.png" style="width:3.18463in;height:3.61345in" />

1.5. Configure security settings for each user.

1.6. Grant "Everyone" user permissions (Get Properties, Get Permissions,
Send Message).

1.7. Grant "System" user permissions (Full Control, Delete, Receive
Message, Peek Message, Receive Journal Message, Get Properties, Set
Properties, Get Permissions, Set Permissions, Take Ownership, Send
Message).

1.8. Grant "Infraskope servers" user permissions (Full Control, Delete,
Receive Message, Peek Message, Receive Journal Message, Get Properties,
Set Properties, Get Permissions, Set Permissions, Take Ownership, Send
Message).

1.9. Grant "Administrators" user permissions (Full Control, Delete,
Receive Message, Peek Message, Receive Journal Message, Get Properties,
Set Properties, Get Permissions, Set Permissions, Take Ownership, Send
Message).

1.10. Restart the MSMQ service.

1.11. Start the Infraskope server service.

1.12. Verify that Event ID "9448" entries are appearing in Event Viewer.
