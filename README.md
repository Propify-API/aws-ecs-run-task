# AWS ECS Run Task

Runs a one-off task on AWS ECS Fargate, waits for it to stop, and fails the step unless the target container exits with code 0.

The action runs on Node 24 (`runs.using: node24`). Reference it by an exact tag, such as `@v2.0.0`, so changes to `master` don't reach callers until they opt in.

Usage
``` yaml
- name: Run migration
  uses: Propify-API/aws-ecs-run-task@v2.0.0
  with:
    cluster: staging
    task-definition: run_migration_task_def
    subnets: sb-123123
    security-groups: sg-1231231
    targetContainerName: migrations

- name: Run more Migrations
  uses: Propify-API/aws-ecs-run-task@v2.0.0
  with:
    cluster: staging
    task-definition: server_backend_task_def
    subnets: sb-123123
    security-groups: sg-1231231
    assign-public-ip: DISABLED
    targetContainerName: server
    override-container: server
    override-container-command: |
        sh
        -c
        cd database && python migrate.py
```

See [action.yml](action.yml) for the full documentation of this action's inputs and outputs.

Note: `task-definition` takes the name or ARN of an existing task definition. To use a task definition file, register it first, for example with `aws-actions/amazon-ecs-deploy-task-definition`.

## Development

After you change `index.js`, rebuild the bundled `dist/` and commit it:

``` sh
npm ci
npm run package
```

## Credentials and Region

This action relies on the [default behavior of the AWS SDK for JavaScript v3](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/setting-credentials-node.html) to determine AWS credentials and region.
Use [the `aws-actions/configure-aws-credentials` action](https://github.com/aws-actions/configure-aws-credentials) to configure the GitHub Actions environment with environment variables containing AWS credentials and your desired region.

## License Summary

This code is made available under the MIT license.
