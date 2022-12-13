# novo-projeto-ade
Alguma coisa para teste
 This is a basic workflow to help you get started with Actions. It's copied from the GitHub Graduation Workflow.

name: GitHub PR - Patch Request


# Controls when the action will run.
on:
  # Triggers the workflow on push or pull request events but only for the main branch
  pull_request_target:
    types:
      - opened
      - review_requested
      - ready_for_review
  workflow_dispatch:
jobs:
  build:
    runs-on: ubuntu-latest
    defaults:
      run:
        shell: bash
        working-directory: .github/workflows/src
    name: Getting grades
    steps:
      - uses: actions/checkout@v2
      - name: Setup node
        uses: actions/setup-node@v2
        with:
          node-version: 14
      - name: Cache modules
        id: cachemodules
        uses: actions/cache@v2
        with:
          path: .github/workflows/src/node_modules
          key: ${{ runner.os }}-${{ secrets.CACHE_VERSION }}-${{ hashFiles('.github/workflows/src/package.json') }}
      - name: Setup npm
        run: npm install -g npm
      - name: Install dependencies
        if: steps.cachemodules.outputs.cache-hit != 'true'
        run: npm install
      - name: Get Minute
        id: getminute
        run: echo "::set-output name=minute::$(date -u '+%M')"
        shell: bash # For Windows, Linux and macOS default to bash alrea

          
      - name: run validations
        run: node index.js
        env:
          GH_SECRET: ${{ secrets.GH_SECRET }}
