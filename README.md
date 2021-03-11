# Github Action Workflow Examples

## Usage

### Sharing Github Action Workflows within Organization

There are a few steps to create Github Action templates for an organization.

1. Create a new public repository named `.github` in the organization
2. Create a directory named `workflow-templates` within the `.github` repo
3. Create a new workflow file inside the `workflow-templates` directory. For example, I created a workflow file named `angular-ci.yml` inside the `workflow-templates` folder

```yaml
# A workflow template for Angular project

name: CI

# the placeholder `$default-branch` will be automatically replaced with
# the name of the repository's default branch, e.g. main
on:
  push:
    branches: [$default-branch]
  pull_request:
    branches: [$default-branch]

jobs:
  test:
    name: Test and Build
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node: [10.x]
    steps:
      - name: Checkout Code
        uses: actions/checkout@v2

      - name: Use Node.js ${{ matrix.node }}
        uses: actions/setup-node@v2
        with:
          node-version: ${{ matrix.node }}

      - name: Install dependencies
        run: npm ci

      - name: Lint
        run: npm run lint

      - name: Test
        run: npm run ng -- test --watch=false --progress=false --sourceMap=false --browsers=ChromeHeadless
```

4. Create a metadata file inside the `workflow-templates` directory. The metadata file must have the same name as the workflow file, and it must be appended with `.properties.json`. For example, I created this file `angular-ci.properties.json` that describes my workflow template `angular-ci.yml` within the `workflow-templates` folder.

```json
{
  "name": "Angular CI Workflow",
  "description": "CI workflow template for Angular projects.",
  "iconName": "example-icon",
  "categories": ["TypeScript"],
  "filePatterns": ["package.json$"]
}
```

## Using a workflow template from your organization

1. Those who want to use the Github Actions template can navigate to their repository's main page and click on the `Actions` tab.

2. Developers can choose a workflow template from the organization and click **Set up this workflow.**

3. As soon as the change is committed and pushed to the default branch (`master` in this example), the workflow will run automatically.
