# GitHub | Spinnaker Status Notification Gating Plugin
Goal: Allow users to configure targetted blocking pipeline stage notifications for specific stages using Status updates

## Notes and architecture recommendations from GitHub & Armory about this project
- Example: people cannot allow merges into a master branch if the current dev package was not successfully deployed
- Specific example: case around integration testing to validate that an API got updated. Let's say you're writing selenium API deployments to do canary analysis or E2E integration testing. If the canary comes back with a fail, this could send a status back - your memory utilization went to 90% over the baseline, so we're not going to let you merge this code into master.
