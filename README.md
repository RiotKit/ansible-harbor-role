Dockerized Project
==================

Automated deployment of Dockerized environment from GIT which includes:

- systemd integration
- production specific .env configuration
- test server specific .env configuration (when: host equals localhost)
- Makefile support to manage the project
- Optionally: Encrypted .env-prod files in project repository, decrypted during deployment

Preliminaries
-------------

1. You need a project at any git server.
2. The project need to have a Makefile with task "deployment_pre"
3. Your project need to have an `.env` file, or `.env-template` that will be copied into `.env`

Environment
----------- 

```yamlex 
# user who deploys the application
deploy_user: tech.admin
deploy_group: technical

project_dir: /project

# change it to sg. "sudo ./make.sh" if you have a root-executed entrypoint for given group of users
make_executable: "make"

# this origin will be set only during deployment time
git_deploy_url: https://user:password@somehost/someuser/somerepo

# without user and password, this form will stay on server
git_regular_deploy_url: https://somehost/someuser/somerepo

# prevents from restarting whole environment when deploying the project over and over again
avoid_whole_environment_restart: yes

# prefer .env-prod (if present in cloned git repository) instead of .env-dist
prefer_env_prod_over_dist: yes

# .env support (also works with .env-prod)
test_specific_env:
    - { line: "DOMAIN_SUFFIX=.localhost", regexp: '^DOMAIN_SUFFIX', title: 'env: Add domain suffix - .localhost' }

production_specific_env:
    - { line: "DOMAIN_SUFFIX=", regexp: '^DOMAIN_SUFFIX', title: 'env: Remove domain suffix' }

```
