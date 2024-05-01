# DDEV Sites

GitHub Action for deploying a site with DDEV.

DDEV is a tool for launching multiple sites on a single server using Docker.

This action makes it easy to clone a site and launch `ddev start`.

Installation
------------
This action does NOT install `ddev`. 

If using on a self-hosted runner, make sure you install ddev first.

If running in CI, you can install DDEV in github workflows with this action: https://github.com/Lullabot/drainpipe/blob/main/scaffold/github/actions/common/ddev/action.yml

Usage
-----

Create your Github Workflow configuration in `.github/workflows/ci.yml` or similar.

```yaml
name: Pull Requests
on: [pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: operations-platform/ddev-site:v1
      with:
        # Set to "yes" to run the sync-command.
        sync: yes
        ssh-known-hosts: ${{ secrets.SSH_KNOWN_HOSTS }}
        ssh-private-key: ${{ secrets.SSH_PRIVATE_KEY }}
       
    # ... then your own project steps ...
```

