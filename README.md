# .github
GitHub organization for Massif Press, creator of the Lancer RPG.

# LCP Deployment Workflows
This repository includes several [workflows](https://github.com/massif-press/.github/tree/master/workflows) to streamline the deployment of LCP packages to NPM and Itch.io upon the creation of a GitHub Release. LCPs are data files that can be used with [COMP/CON](https://compcon.app/), the character creation app for the [Lancer RPG](https://massifpress.com/lancer). It additionally includes a workflow that increments the LCP and package versions and creates a matching GitHub tag.

## Secrets Required
Deployment to NPM and Itch.io requires the following GitHub secrets, respectively:
* `NPM_TOKEN`: NPM access token tied to the target NPM account ([documentation here](https://docs.npmjs.com/creating-and-viewing-access-tokens)).
* `BUTLER_CREDENTIALS`: Butler API key tied to the target Itch.io account ([documentation here](https://itch.io/docs/butler/login.html)).

## Variables Required
Deployments to NPM and Itch.io require several preset GitHub variables, either at the org, repository, or environment level.

NPM deployments require the following variables:
* `ACCESS_LEVEL`: Should be either `public` or `restricted`. Used to determine NPM package visibility as per `npm publish --access`.

Itch.io deployments require the following variables:
* `ITCH_GAME`: Itch.io project name. If your project URL is `username.itch.io/project-name`, your project name is `project-name`.
* `ITCH_USER`: Itch.io user name. If your project URL is `username.itch.io/project-name`, your user name is `username`.
* `BUTLER_CHANNEL`: Project channel name for the package in `kebab-case`. Packages submitted to a project under an existing channel name overwrite the previous package with the same channel name. Options for packages in that channel (like "mark as demo" or "hide this project") are controlled via the project's "Edit project" page on Itch.io; they are maintained between deployments. For more information, check the [Butler docs](https://itch.io/docs/butler/pushing.html).

# Intended Workflow
1. The `bump-version-tag.yml` workflow is intended to be used whenever it's time to increment the LCP's version due to a patch or otherwise. This tag does *not* need to be intended for release.
2. The `butler-publish.yml`, `npm-publish.yml`, and `massif-version-update.yml` are intended to be run whenever a new GitHub Release is created for a tag. They do not all need to be run; if a given project only needs to deploy to itch.io, but doesn't exist on NPM and isn't featured on COMP/CON's community LCP list, the caller job only needs to call the `butler-publish.yml` workflow.
