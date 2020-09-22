# Project 1: GitHub Apps for Auth within Spinnaker
Goal: move to a Spinnaker App Auth model with GitHub to remove PATS, which aren't desirable from a security perspective
This is a first priority since it acts as a logical prerequisite to the others (which would require GitHub Auth)

## Notes and architecture recommendations from GitHub & Armory about this project

### Problem statement
- @continuouslee's feedback from customers: when at GH, when GitHub Apps were first being developed before Actions, Apps helped cutomers who were creating PATs when using tools like Jenkins. These PATs which allowed people to perform actions on others behalf caused difficulties and mystery errors after offboarding
- Current flow for Spinnaker onboarding involves AuthZ to GitHub, Enterprise Cloud, SAML, and set up with teams in GH
-Notes on user experience for creating and configuring an individual Spinnaker app
  - New solution should retain AuthZ for managing Spinnaker auth
  - May be important to plan around Spinnaker SaaS
  - 100 instances of Spinnaker all wanting to communicate to one GH instance through an app may require creating an individual Spinnaker app for each instance, but this depends on what we need access to
  - Webhook and oauth URLs will be different for each one. each single tenant spinnaker instance can receive webhooks
  - A feature is built into apps to send our webhooks to the URL configured in the app settings, but in a normal gh app model with marketplace install, that is referring to a single SaaS instance.

### Proposed workflows
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
    - __The team determined that this is likely the best proposed architecture__
    - generate a custom app manifest to get the keys that are needed, and this can be used to generate a spinnaker operator config
    - Sample instructions: "we have created you an app manifest in a repo for you, and spinnaker operator in the repo. take these, and apply the app manifest to your org, and then apply the operator config to your spinnaker instance"
    - App manifest flow is not seamless and there are a few complaints about it, but users just need to click a button that sends a request, then it walks you through creating an app. we can suggest an app name to further automate
    - [App manifest flow automation](https://github.com/imjohnbo/app-manifest-flow)
    - [Creating an app from manifest documenation](https://docs.github.com/en/developers/apps/creating-a-github-app-from-a-manifest)
