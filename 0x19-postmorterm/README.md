Issue Summary
On August 21st, 2024, from 09:00 UTC to 10:10 UTC, our internal ticketing system experienced a major outage. Users were unable to create, update, or view tickets, leading to a complete disruption in customer support services. Approximately 75% of users were affected, resulting in a backlog of unprocessed tickets and a significant increase in customer wait times. The root cause of the outage was an unexpected database failure triggered by a misconfigured scheduled backup job that overloaded the system.

Timeline
- 09:00 : Monitoring alert notified the engineering team of increased latency in the ticketing system.
- 09:02 : On-call engineer started investigating the database performance.
- 09:10 : Early assumption pointed to a possible issue with a recent code deployment.
- 09:15 : Rolled back the latest deployment, but the issue persisted, ruling out a code-related issue.
- 09:25 : Escalated the incident to the database team for further investigation.
- 09:30 : Database logs indicated an overload due to concurrent backup jobs running unexpectedly.
- 09:45 : Misconfiguration in the backup scheduling was identified as the cause.
- 10:00 : Backup job was terminated and the database was restarted.
- 10:10 : Full service was restored, and users were able to access the ticketing system without further issues.

Root Cause and Resolution
The outage was caused by a misconfigured cron job intended to run database backups. Instead of running during off-peak hours, the backup process was triggered during peak operational hours due to a timezone misconfiguration in the cron job settings. As a result, the database became overwhelmed by the backup process, leading to degraded performance and eventual failure. The ticketing system, which relies heavily on database operations, was unable to function, resulting in the outage.

To resolve the issue, the engineering team identified the cron job as the source of the problem by reviewing database logs. Once the issue was isolated, the backup process was terminated, and the database was restarted to restore normal operation. The cron job configuration was corrected, ensuring that backups are now scheduled to run during non-peak hours.

Corrective and Preventative Measures
To prevent similar incidents in the future, the following corrective and preventative measures will be implemented:

- Improvements:
  - Enhance monitoring to detect unexpected spikes in database load related to backup processes.
  - Review and document all cron job schedules to ensure they are correctly configured and aligned with non-peak hours.

- Specific Tasks:
  - Implement automated alerts for failed or misconfigured cron jobs.
  - Update cron job scheduling to run during maintenance windows and ensure proper timezone settings.
  - Conduct a thorough review of all database operations and backup processes to identify and address potential bottlenecks.
  - Test backup procedures in a staging environment before applying changes to production.


