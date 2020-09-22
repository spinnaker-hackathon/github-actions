# Project 3: Echo | GitHub Integration Plugin for bi-directional communication with Spinnaker
Goal: Pick up existing plugin project. @continuouslee built a [Spinnaker plugin](https://github.com/leefaus/echo-github-plugin) that ties into [Echo](https://github.com/spinnaker/echo) to talk to the Deployments API to get deployment data including status

## Notes and architecture recommendations from GitHub & Armory about this project

- In the case that users can leverage GitHub Actions to build and publish artifacts, this plugin should make it possible to use Spinnaker to deploy with a bi-directional communication between GitHub and Spinnaker
- Built on the GitHub App framework, which allows you to connect an integration without doing things like sending clear text username/passwords or having to create a personal access token
- Next steps for this plugin should be to get the metadata from a webhook (head branch, head sha, pull request info, etc.) and pass it into the plugin
