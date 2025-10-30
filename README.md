# DDEV Hosting Reusable GitHub Actions
## Self-hosted automation for ddev projects

These reusable GitHub Workflows allow you to automate the deployment and testing of DDEV sites on your own servers.

Useful for hosting and CI/CD servers.

## Reusable Workflows

Include these workflows inside your own github workflow files:

- [`operations.site.deploy.yml`](./.github/workflows/operations.site.deploy.ddev.yml)
  - **Deploy Code:** git clone and checkout desired branch to desired path.
  - **Start Site:** Write special DDEV configs and run `ddev start` to launch the site.
  - **Import Site:** Run your own sync command to import or install your site.
- [`operations.site.destroy.yml`](./.github/workflows/operations.site.destroy.ddev.yml)
  - **Remove Site**: `ddev rm -OR`
  - **Remove Code**: `rm -rf $DIR`
- More workflows TBD.

## Server Setup

To prepare a server for running these workflows, you can use the [Operations Site Runner]([url](https://github.com/operations-project/ansible-collection-site-runner)) Ansible collection. see https://github.com/operations-project/ansible-collection-site-runner.

The main components:

- Sysadmin users from GitHub accounts.
- Platform user for running sites.
- Control (sudo) user for configuring server.
- Docker
- DDEV
- GitHub runners, one per repository.

Usage
-----

Copy the `example.*.yml` workflows located at [.github/workflows/](.github/workflows) to your projects `.github/workflows` folder.

Pull Requests and Live Environments must be handled in separate files, so that live sites only deploy on specific branches.

For a complete example, see [.github/workflows/example.site.preview.yml](.github/workflows/example.site.preview.yml)

# ... then your own project steps ...
```

