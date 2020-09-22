# GitHub Bake Artifact Management Plugin 
Goal: Move packages between versions using the workflow dispatch model
- Given a Spinnaker app enabled inside the configuration, users can do more closely-knit round robin actions - for example, a github action, through to a build, with the ability to store the artifact in a GH registry. Then in the registry there is a notification that the package is there, which triggers a Spinnaker workflow, which reports back via the GitHub depoyments API, to show where the deployment is for a particular PR. If we can also provide info on ingress controller (external DNS entry for example) - then boom, here's what I just deployed.
- Look for opportunities to set up more SDLC tasks through actions where there is dual representation of the data. For example, Actions to report relevant into Spinnaker for policy action, such as that an artifact is a certified docker image (that is exposed as part of a Spinnaker workflow)
- Customer workflow: devs are creating docker images in dev/test environments with tags that use a build # or something similar at the end of the name string. But when they want to move to prod, they will tag the images like releases with a different convention.
  - Proposed Actions: given a pipeline that produces an artifact inside of spinnaker, user marks the artifact as the thing they want to use in prod
  - Rebuild the image based on the sha from a particular PR as a prod release
- Architecture plan:
  - Leverage workflow dispatch. [Repo & Workflow Dispatch documenation](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#repository_dispatch)
  - Repository dispatch or workflow distpatch will arbitrarily kick of a workflow and then trigger events when the wf is complete
