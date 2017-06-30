# cloud-init QA Scripts

This is a collection of helpful scripts to speed up testing.

## TODO

The hope is to have a series of shell scripts for each cloud to interact
with their own tools or directly with the clouds to speed up deployment
and common actions taken by QA. For example:

 * Deploy on a cloud instance on specific region and type
 * Update from proposed, reboot, collect specific data
 * Deploy with specific storage or networking settings
 * Analyze logs for warn or error messgaes
 * Analyze logs for exceptions
 * Collect all required logs for triage and debug
