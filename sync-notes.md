# Sync notes from meetings on the GitHub Challenge

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
 - We would love to call this the "GitHub Challenge" at the Spinnaker Summit Gardening hackathon! @John is going to talk with the DevRel org at GitHub to confirm this is OK and find out how they can participate
- Goal: First class integration between Spinnaker and GitHub Actions
 
#### Proposed Spinnaker | GitHub integration components
- Subgoals:
  - Provide self-service enablement for appdev teams
  - Improve the authorization model between Spinnaker & GitHub to make the interaction more secure in improve on/off-boarding
  - Provide visibility into the interactions going on outside with a PR after it's merged. After merge into master, notify back to ultimately say, here this artifact is now in this specific environment.
  - Integrate Actions with Spinnaker's VM bake model
  - Integrate with package registry to move packages between sw versions 
  - Expand integration with the Deployments API for bi-directional communication between GitHub | Spinnaker

#### GitHub Apps for Auth within Spinnaker 
- Goal: move to a Spinnaker App Auth model with GitHub to remove PATS, which aren't desirable from a security perspective
- This is a first priority since it acts as a logical prerequisite to the others (which would require GitHub Auth)
- Problem statement
    - Lee's feedback from customers: 
      - when at GH, when GitHub Apps were first being developed before Actions, Apps helped cutomers who were creating PATs when using tools like Jenkins. These PATs which allowed people to perform actions on others behalf caused difficulties and mystery errors after offboarding
    - Current flow for Spinnaker onboarding involves AuthZ to GitHub, Enterprise Cloud, SAML, and set up with teams in GH
- Notes on user experience for creating and configuring an individual Spinnaker app
  - New solution should retain AuthZ for managing Spinnaker auth
  - May be important to plan around Spinnaker SaaS
  - 100 instances of Spinnaker all wanting to communicate to one GH instance through an app may require creating an individual Spinnaker app for each instance, but this depends on what we need access to
  - Webhook and oauth URLs will be different for each one. each single tenant spinnaker instance can receive webhooks
  - A feature is built into apps to send our webhooks to the URL configured in the app settings, but in a normal gh app model with marketplace install, that is referring to a single SaaS instance.
  - Manual Workflow: 
    - go through the app manifest flow and identify the permissions and events needed in yaml or json
    - make a post to a url at gh.com - this will walk people through creating their own spinnaker app
    - app manifest workflow hasn't changed much [since Lee was at GitHub], and this isn't the most seamless integration
    - it's easy for folks to create their own private app that won't be going to the marketplace - it's their own instance
    - if spinnaker has the right endpoints set up and there is good doc, this could work
- Can we introduce more automation?
  - With OAuth, you can automatically create register and install the app
  - Some central entity such as Armory could host an OAuth app which creates a single entry point, making it easy to configure a dedicated instance for an org - 
  - Could have a series of GH Actions that could do this on our behalf - but the problem is you need higher privileges than the Actions app will allow you to
  - Another option: lightest touch without security interference over privileges
    - generate a custom app manifest to get the keys that are needed, and this can be used to generate a spinnaker operator config
    - Sample instructions: "we have created you an app manifest in a repo for you, and spinnaker operator in the repo. take these, and apply the app manifest to your org, and then apply the operator config to your spinnaker instance"
    - App manifest flow is not seamless and there are a few complaints about it, but users just need to click a button that sends a request, then it walks you through creating an app. we can suggest an app name to further automate
    - [App manifest flow automation](https://github.com/imjohnbo/app-manifest-flow)
    - [Creating an app from manifest documenation](https://docs.github.com/en/developers/apps/creating-a-github-app-from-a-manifest)

#### GitHub Bake Artifact Management Plugin 
Goal: Move packages between versions using the workflow dispatch model
- Given a Spinnaker app enabled inside the configuration, users can do more closely-knit round robin actions - for example, a github action, through to a build, with the ability to store the artifact in a GH registry. Then in the registry there is a notification that the package is there, which triggers a Spinnaker workflow, which reports back via the GitHub depoyments API, to show where the deployment is for a particular PR. If we can also provide info on ingress controller (external DNS entry for example) - then boom, here's what I just deployed.
- Look for opportunities to set up more SDLC tasks through actions where there is dual representation of the data. For example, Actions to report relevant into Spinnaker for policy action, such as that an artifact is a certified docker image (that is exposed as part of a Spinnaker workflow)
- Customer workflow: devs are creating docker images in dev/test environments with tags that use a build # or something similar at the end of the name string. But when they want to move to prod, they will tag the images like releases with a different convention.
  - Proposed Actions: given a pipeline that produces an artifact inside of spinnaker, user marks the artifact as the thing they want to use in prod
  - Rebuild the image based on the sha from a particular PR as a prod release
- Architecture plan:
  - Leverage workflow dispatch. [Repo & Workflow Dispatch documenation](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#repository_dispatch)
  - Repository dispatch or workflow distpatch will arbitrarily kick of a workflow and then trigger events when the wf is complete

#### Echo | GitHub Integration Plugin for bi-directional communication with Spinnaker
Goal: Pick up existing plugin project. @continuouslee built a [Spinnaker plugin](https://github.com/leefaus/echo-github-plugin) that ties into [Echo](https://github.com/spinnaker/echo) to talk to the Deployments API to get deployment data including status
- In the case that users can leverage GitHub Actions to build and publish artifacts, this plugin should make it possible to use Spinnaker to deploy with a bi-directional communication between GitHub and Spinnaker
- Built on the GitHub App framework, which allows you to connect an integration without doing things like sending clear text username/passwords or having to create a personal access token
- Next steps for this plugin should be to get the metadata from a webhook (head branch, head sha, pull request info, etc.) and pass it into the plugin


#### GitHub | Spinnaker Status Notification Triggers Plugin
Goal: Allow users to configure targetted blocking pipeline stage notifications for specific stages using Status updates
- Example: people cannot allow merges into a master branch if the current dev package was not successfully deployed
- Specific example: case around integration testing to validate that an API got updated. Let's say you're writing selenium API deployments to do canary analysis or E2E integration testing. If the canary comes back with a fail, this could send a status back - your memory utilization went to 90% over the baseline, so we're not going to let you merge this code into master.
