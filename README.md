# Backup folder status

This folder is **not a backup system** and is not a restorable application.
It contains only standalone visualization/demo assets; no backup source,
manifest, data format, restore command, retention policy, or owner is defined.

Do not place credentials, database dumps, customer data, or production
artifacts here. The existing `.gitignore` is defensive only and does not turn
this folder into an approved backup destination.

An owner must decide whether to archive these demo assets or move them into a
clearly identified application with documented provenance. A real backup
system must be designed separately with encryption, retention, access control,
integrity checks, and tested restoration.
