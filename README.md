# pushpreview-action

The `pushpreview-action` is a GitHub Action designed to automate the process of creating previews within the [PushPreview](https://pushpreview.com) platform. This action takes a specified source directory, compresses it, and sends it to PushPreview, where a preview is generated. It is particularly useful for developers and teams who want to streamline their workflow by automatically generating previews for their web projects or applications.

## Inputs

This action requires the following inputs:

1. **source-directory**
   - **Description**: The directory to compress and send.
   - **Required**: Yes

2. **github-token**
   - **Description**: The secret token of GitHub.
   - **Required**: Yes

3. **pushpreview-token**
   - **Description**: The secret token for the PushPreview API.
   - **Required**: Yes

## Outputs

The action provides the following output:

- **comment**
  - **Description**: PushPreview comment with links for every change edited and the URL of the preview.

## Secrets

The action uses the following secrets:

- `GITHUB_TOKEN`: This is a GitHub secret used to authenticate and interact with GitHub APIs.
- `PUSHPREVIEW_TOKEN`: This is a secret token for the PushPreview API, necessary for authentication and authorization.

## Example usage

Below is an example of how to use the `push-preview-action` in a workflow:

```yaml
name: PushPreview

on:
  issue_comment:
    types:
      - created
jobs:

  comment:
    runs-on: ubuntu-latest
    if: github.event_name == 'issue_comment' && github.event.comment.body == '/preview'
    steps:
      - uses: actions/checkout@v3          

      - uses: actions/setup-node@v3
        with:
          node-version: 18
      
      - name: Install dependencies
        run: cd docs && yarn install --frozen-lockfile
      
      - name: Build website
        run: cd docs && yarn build

      - name: Generate preview
        uses: PushLabsHQ/pushpreview-action@1.0.0
        with:
          source-directory: ./docs/build
          github-token: ${{ secrets.GITHUB_TOKEN }}
          pushpreview-token: ${{ secrets.PUSHPREVIEW_TOKEN }}
```
