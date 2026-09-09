# git-config

GitHub action to configure the git user corresponding to the `GITHUB_TOKEN` environment variable.

The action configures the git user name and email:

```
git config --global user.name <name>
git config --global user.email <email>
```

## Usage

Here's an example of how to use the action in a workflow to configure the git user.
After you configure the user, you can make changes and push them.

```yaml
name: Create new file

permissions:
  contents: write

on:
  workflow_dispatch: # Allow manual triggers

jobs:
  my-job:
    name: My job
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
      - id: git-author
        name: Configure git user from GitHub token
        uses: MarcoIeni/git-config@v0.1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      - name: Commit changes
        run: |
          echo "${{ steps.git-author.outputs.email }}"
          echo "${{ steps.git-author.outputs.name }}"
          touch new-file
          git add .
          git commit -m "Create new file"
          git push
```

<br>

<sup>
Licensed under either of <a href="LICENSE-APACHE">Apache License, Version 2.0</a>
or <a href="LICENSE-MIT">MIT license</a> at your option.
</sup>

<br>

<sub>
Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall be
dual licensed as above, without any additional terms or conditions.
</sub>

## Privacy

This Action contacts Chainguard's licensing server to verify authorization. Connection metadata (IP address, GitHub repository identifier, timestamp, and any metadata encoded in the auth token) is transmitted to Chainguard, Inc. even if authorization is denied in accordance with our [Privacy Notice](https://www.chainguard.dev/legal/privacy-notice)
