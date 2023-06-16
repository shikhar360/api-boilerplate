## This is the boilerplate for making a package with ci integration

## Things need to do to apply the changesets
---- terminal
`pnpm add -D @changesets/cli`

----package.json
Add a release command 
```
"release" : "pnpm run lint && pnpm run test && pnpm run build && changeset publish"

```

Add the publish.yml file 

```yml
name: Publish
on:
  push:
    branches:
      - "main"
concurrency: ${{ github.workflow }}-${{ github.ref }}

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: pnpm/action-setup@v2
        with:
          version: latest
      - uses: actions/setup-node@v3
        with:
          node-version: 18.x
          cache: "pnpm"
      - run: pnpm install --frozen-lockfile
      - name: Create Release Pull Request or Publish
        id: changesets
        uses: changesets/action@v1
        with:
          publish: pnpm run release
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        NPM_TOKEN: ${{ secrets.NPM_TOKEN }}

```

Needs to add the secret to the `"Settings" > "Secrets" > "New repository secret".` in the github.


