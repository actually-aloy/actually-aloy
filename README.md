name: Generate GitHub stats

on:
  schedule:
    - cron: "0 0 * * *"   # once a day
  workflow_dispatch:        # lets you trigger it manually from the Actions tab

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - name: Generate stats card
        uses: jstrieb/github-stats@master
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          exclude_repos: ""
        continue-on-error: true

      - name: Commit generated files
        run: |
          git config --global user.name "github-actions[bot]"
          git config --global user.email "github-actions[bot]@users.noreply.github.com"
          git add -A generated/ || true
          git diff --staged --quiet || git commit -m "chore: update stats [skip ci]"
          git push
