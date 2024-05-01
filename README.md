# Operations Site Server: DDEV GitHub Action

This GitHub Action is designed to host real live sites using self-hosted GitHub runners and DDEV.

DDEV is a tool for launching multiple sites on a single server using Docker.

This action makes it easy to clone a site and launch `ddev start`.

Notes
-----

This action does NOT install `ddev`. 

If using on a self-hosted runner, make sure you install ddev first.

If running in CI, you can install DDEV in github workflows with this action: https://github.com/Lullabot/drainpipe/blob/main/scaffold/github/actions/common/ddev/action.yml

Operations Site Server
----------------------

You can prepare a server for running sites using the Operations Site Server tool: https://github.com/operations-platform/site-server/

It will prepare server users, install DDEV, and setup GitHub Runners as a service.

Usage
-----

Create your Github Workflow configuration in `.github/workflows/ci.yml` or similar.

Pull Requests and Live Environments should be handled separately
```yaml
# ./.github/workflows/pull-requests.yml
name: Pull Requests
on: [pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: operations-platform/ddev-site:v1
      with:
        # Set to "yes" to run the "sync-command". THIS WILL DESTROY THE SITE DATA.
        sync: "yes"

        # The command to run to sync up the site with data. 
        # The default (shown) assumes you have a drush alias of @live.
        sync-command: "ddev drush sql:sync @live @self"

        # Add SSH information to GitHub secrets to connect to remote servers.
        # Command to get SSH_KNOWN_HOSTS:
        # ssh-keyscan -H yourliveserver.com -H github.com 
        ssh-known-hosts: ${{ secrets.SSH_KNOWN_HOSTS }}
        
        # Put a private key in secrets and in the live sites `.ssh/authorized_keys` file.
        ssh-private-key: ${{ secrets.SSH_PRIVATE_KEY }}
        
        # The DDEV `project_tld` config option determines the URLs that are created for the sites.
        # You can set the TLD here, or use the GitHub Runner user's global ddev config.
        ddev-project-tld: "ci.myserver.com"

        # A list of domains to apply to this environment. Must be a string because of github actions.
        ddev-fqdns: |
          - www.thinkdrop.net

# ... then your own project steps ...
```

