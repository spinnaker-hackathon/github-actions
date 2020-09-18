# Github-Actions & Spinnaker

GitHub is collaborating with the Spinnaker community to build a first class GitHub Actions CI integration similar to Spinnaker's integrations with Jenkins, Google Cloud Build, and AWS CodeBuild. This would allow users to leverage Actions as part of a "bake" stage, similar to Jenkins, and would expose Actions artifacts and GitHub Package Registry as artifacts in Spinnaker pipelines. 

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


## Sync Notes 9/4
- Goal: make GitHub part of the Bake stage, more native as part of Spinnaker
  - Avoid creating GitHub actions that will be replaced by an integration anyway
  - Install a GitHub App on your org to facilitate build artifact use in pipelines
- Unknowns: how big of a lift is the proposed project to create a CI integration
- Could this be built as a plugin? Probably should be!
  - Blog post about plugins: https://blog.spinnaker.io/spinnakers-extensibility-reaches-new-heights-with-plugins-645fd73f8d6a
  - Doc: https://spinnaker.io/guides/developer/plugin-creators/
- Question - Auth into single tenant Spinnaker instance from GitHub?
  - Need to move away from PAT authentication and start using OAuth App or GitHub App instead
  - Setup to make sure that the GitHub app can talk to Spinnaker. This app can be used to watch for events, etc.
- Is [GitHub Container Registry](https://github.blog/2020-09-01-introducing-github-container-registry/) compatible with Spinnaker? If not, let's give that feedback to the GitHub team.
- Ige's thoughts:
  - he's using GitHub actions with Docker currently 
  - provided suggestions for actions in the [feature requests list](#spinnaker-feature-requests-for-github-and-actions) to help automate software delivery workflows in Spinnaker
  - interested in working on this project
- Next steps:
  - John & Rosalind: Invite Armory and GitHub engineers to next meeting
  - All: Share this readme with interested parties/potential contributors to further vet the proposal
  - Rosalind: Loop in Abhilash Gnan, another hackathon volunteer, to this doc & meeting invite
  - Rosalind: Update hackathon repo with info about the Fall event so it's ready to share with collaborators
  
 ## Sync Notes 9/18
 - We would love to call this the "GitHub Challenge" at the Spinnaker Summit Gardening hackathon! @John is going to talk with the DevRel org at GitHub
 
  
