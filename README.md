# get-git-tag

This action gets tag name from commit that triggered the action and puts it into an environment variable GIT_TAG_NAME.  It will also export is as an output named "tag".

You can also use optional parameters `tagRegex` and `tagRegexGroup` to extract a part from tag string.

<a href="https://github.com/little-core-labs/get-git-tag"><img alt="GitHub Actions status" src="https://github.com/little-core-labs/get-git-tag/workflows/Tests/badge.svg"></a>

Forked from [olegtarasov/get-tag](https://github.com/olegtarasov/get-tag) for maintenance.

## Usage

### Pre-requisites
Create a workflow `.yml` file in your repositories `.github/workflows` directory. An [example workflow](#example-workflow) is available below. For more information, reference the GitHub Help Documentation for [Creating a workflow file](https://help.github.com/en/articles/configuring-a-workflow#creating-a-workflow-file).

### Inputs

```yaml
tagRegex:
    description: 'Regex to capture a group text as tag name. Full tag string is returned if regex is not defined.'
    default: ''
  tagRegexGroup:
    description: 'Regex group number to return as tag name.'
    default: '1'
```

### Outputs

```yaml
tag:
  description: The tag name.
```

### Example workflow

```yaml
    steps:
      - uses: little-core-labs/get-git-tag@v3.0.1
        id: tagName
        with:
          tagRegex: "foobar-(.*)"  # Optional. Returns specified group text as tag name. Full tag string is returned if regex is not defined.
          tagRegexGroup: 1 # Optional. Default is 1.
      - name: Some other step # Output usage example
        with:
          tagname: ${{ steps.tagName.outputs.tag }}
      - name: Yet another step # Environment variabl usage example
        run: |
          docker build . --file Dockerfile --tag docker.pkg.github.com/someimage:$GIT_TAG_NAME
```

## FAQ

### Can you offer a major version tag/branch alias?  I want automatic updates!

Nope!  This was always weird/bad pattern of github actions and I don't have time to maintain or automate that.  Luckily github offers a solution for this.  Create a `.github/dependabot.yml` with, at a minimum, the following config:

```yaml
# Basic dependabot.yml file with
# minimum configuration for two package managers

version: 2
updates:
  # Enable version updates for npm
  # Enable updates to github actions
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "daily"
```

Furthermore, this action won't be changing much.
