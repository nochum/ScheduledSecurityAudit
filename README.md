# ScheduledSecurityAudit

Salesforce customers often need to **proactively** monitor changes that affect the overall security of their Salesforce org.  Although Salesforce provides the Setup Audit Trail both as a log file and as an object that can be queried, it is not yet possible to monitor these setting changes via the Transaction Security feature of Event Monitoring.

It is actually fairly straightforward to implement these capabilities using Scheduled Apex that queries the SetupAuditTrail object. Note that the Display and Section fields on the SetupAuditTrail object are not "filterable" and hence cannot be part of a SOQL query. This project contains Apex code that can be scheduled to run at a configurable frequency, and send notifications to one or more users if changes have been made since the last run.

## How It Works

This project contains code that is designed to run at a fixed schedule.  The code leverages a custom setting that contains a single record containing the last run time.  With each run the code queries for changes to the setup audit trail that have taken place since the previous run.  If it encounters changes to IP Ranges, Password Policies, and/or Session Settings, it generates an email that can be configured to be sent to one or more individuals or email aliases.

## What it Covers

The code in this project should provide notifications in the event of any of the following configuration changes:

* IP Ranges
* Password complexity policy
* Password expiration policy
* Password history retention
* Password lockout interval
* Maximum login attempts
* Minimum password length
* Minimum password lifetime
* Password secret answer obfuscation policy
* Password question restriction policy
* Session disable timeout warning policy
* Session enable CSP on email policy
* Session enable CSRF on GET policy
* Session enable CSRF on POST policy
* Session enable cache and autocomplete policy
* Session enable clickjack protection on non-setup Salesforce pages
* Session enable clickjack protection on VF pages with headers policy
* Session enable clickjack protection on VF pages without headers policy
* Session enable clickjack protection on setup pages policy
* Session cross-domain POST requests policy
* Session SMS for one-time PIN delivery policy
* Session IP range enforcement for every request policy
* Session forced logout upon timeout policy
* Session force relogin after Login-As-User policy
* Session locking to domain policy
* Session locking to IP address policy
* Session administrative termination policy
* Session timeout policy

## Scheduling

To schedule this code to run on 15-minute intervals, simply add the Apex class to your org and then run the following in an "Execute Anonymous" window in the Developer Console:

`System.schedule('AuditSecurity-Job1', '0 0 * * * ?', new ScheduledAuditSecurity());`

`System.schedule('AuditSecurity-Job2', '0 15 * * * ?', new ScheduledAuditSecurity());`

`System.schedule('AuditSecurity-Job3', '0 30 * * * ?', new ScheduledAuditSecurity());`

`System.schedule('AuditSecurity-Job4', '0 45 * * * ?', new ScheduledAuditSecurity());`

