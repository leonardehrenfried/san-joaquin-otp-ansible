## trapeze-otp-ansible

Install Trapeze's OTP with ansible

- `make provision`: connects to the host and applies the playbook there

### Graph builds

The script [`build-graph`](roles/otp/templates/build-graph) builds a new graph every night at 1 o'clock.

The script is heavily commented and explains the various steps to clean up and prepare the GTFS feed
for consumption by OTP.

The automatic start is controlled by the systemd timer `graph-build` and if you want to modify
this, then edit `graph-build.timer` and `graph-build.service`.

### Common tasks

**Restarting OTP**

`systemctl restart otp`

This also checks if there are newer images available on dockerhub and downloads
them prior to restarting. 

**Viewing logs**

All logs are sent to `journald` for storage and automatic deletion. Here is
a list of common `journalctl` commands.

- Viewing opentripplanner logs: `journalctl -u otp`
- Viewing graph-build logs: `journalctl -u graph-build`

**Triggering a rebuild of the OTP graph**

`systemctl start graph-build`

A build is run every night but sometimes you want to trigger it manually.

**Debugging a graph build failure**

Check the logs of the graph build from the previous 24 hours:

```
journalctl -u graph-build --since "1 day ago"
```

**aliases**

To see the complete list of useful aliases check out [`alias.sh`](roles/base/templates/alias.sh).

### Further reading

- [GraphQLApi](GraphQLApi.md)
