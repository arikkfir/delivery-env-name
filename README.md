# delivery-env-name

This action takes a given branch name, and returns an appropriate development environment name.

## Rational

Unlike branches, environment names are often used in actual deployment resources like Kubernetes namespaces, Pub/Sub
topics, actual EC2 instance names, etc. Therefore, they are subject to often strict naming conventions (e.g. no special
characters, no more than 63 characters, etc.). Branches, on the other hand, have more relaxed naming conventions (though
there are some restrictions for them as well) - a good example would be the `/` sign which is often used to separate
branch owners - e.g. `joe/special-feature`.

This is where this action comes into play - it takes a given branch name and returns an appropriate "slugged"
environment name which answers most naming restrictions found in the while.

If you find more naming restrictions you believe should be applied - feel free to submit a PR!

## Usage

```yaml
name: Deploy my app
on:
  push:
    branches:
      - '*'
jobs:
  deploy:
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v3
      - id: env                               # allows us to use this step's output in the subsequent step
        uses: arikkfir/delivery-env-name@v1   # runs this action using the latest version from the "v1" branch
        with:
          branch: ${{ github.ref_name }}      # provide the current branch to this action
      - run: |
          echo "I'm about to deploy to: ${TARGET_ENVIRONMENT_NAME}"
        env:
          TARGET_ENVIRONMENT_NAME: ${{ steps.env.outputs.name }} # obtain the environment name from the previous step
```
