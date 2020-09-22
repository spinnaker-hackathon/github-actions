# Github & Spinnaker Integration Projects

GitHub is collaborating with the Spinnaker community to build a first class GitHub integration at Spinnaker Summit Gardening Days! This began with the vision of a CI integration with Actions similar to Spinnaker's integrations with Jenkins, Google Cloud Build, and AWS CodeBuild. This would allow users to leverage Actions as part of a "bake" stage, similar to Jenkins, and would expose Actions artifacts and GitHub Package Registry as artifacts in Spinnaker pipelines. 

In planning the architecture for this, we identified four distinct projects to accomplish a strong integration:

- GitHub App automation for improved token-free auth management for Spinnaker
- [GitHub bake artifact management plugin](https://github.com/spinnaker-hackathon/github-actions/tree/master/github-bake-artifact-plugin)
- [Echo | GitHub integration plugin for bi-directional communication with Spinnaker](https://github.com/spinnaker-hackathon/github-actions/tree/master/github-echo-plugin)
- [GitHub | Spinnaker status notification gating plugin](https://github.com/spinnaker-hackathon/github-actions/blob/master/github-status-gating-plugin)

Each of these projects will be posed to the Spinnaker and wider DevOps community as part of our Fall 2020 Spinnaker Summit Gardening Days, a month-long open source hackathon. Teams can compete to build against any of these project suggestions in the GitHub Challenge category for a chance to win a team prize package. 

We have also identified two open source community-purpose Actions project suggestions for the GitHub Challenge that will benefit both the GitHub and Spinnaker communities:
- GitHub Actions & Forks: Actions should be preserved when private or public repositories are forked. Can we build a workaround? Something that would also automate keeping forks up to date to simplify fork-and-pull-request workflows for OSS contributors who be _excellent_.
- GitHub Actions for Netlify Accounts: Can we devise a workaround for managing Netlify builds for contributors who are unwilling or unable to create a Netlify account and test their PRs themselves? 


## Prior Art
_add any existing GH Actions & Spinnaker integrations here_

- Not an action, but [this plugin](https://github.com/leefaus/echo-github-plugin) calls the GitHub Deployments API. 
  - Use case example: use the API to do a merge of a branch when a deployment to product 
  - [Early video demo](https://youtu.be/2MN-NaOySpo)
- [Actions used for Spinnaker maintenance](https://github.com/spinnaker/scheduled-actions)
  

### Spinnaker Feature Requests for GitHub and Actions

__What GitHub Actions would you like to write or see written to help automate your software devliery workflows in Spinnaker?__

_your answers here_
- Trigger Spinnaker pipelines
- Retrieve artifacts from Spinnaker stages
- Retrieve information from Spinnaker pipeline context
- Retrieve information about a Spinnaker deployment
- Possibly modify these deployments too
- Automate pipeline CRUD ops
- Do things like “Go Get pipeline results and write a comment” (aka an Atlantis kinda thing for Spinnaker execution)

### How would you like to see GitHub and Spinnaker better integrated?
_your answers here_

  
