# CI/CD with React

## Bootstrap the project
- Create a new React app using `create-react-app`

## Setup basic Github Actions for NodeJS
- Add a new Github Actions workflow file (use the Github Actions generator to help you out)

```
name: Node.js CI Example

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:

    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: 16.x

    steps:
    - uses: actions/checkout@v3
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v3
      with:
        node-version: ${{ matrix.node-version }}
        cache: 'npm'
    - run: npm ci
    - run: npm run build --if-present
    - run: npm test
```

## Setup Netlify
- Create a new site on Netlify with "manual deployment" and use an empty folder to create a detached website

## Set environment variables on your Github repository
- Set the `NETLIFY_SITE_ID` as a secret on your Github repository
- Set NETLIFY_AUTH_TOKEN (from [User settings > Applications > Personal access token](https://app.netlify.com/user/applications#personal-access-tokens))

## Add the deployment step on your workflow
Add the following at the end of your workflow file:

```
- name: Deploy to netlify
    uses: netlify/actions/cli@master
    env:
    NETLIFY_AUTH_TOKEN: ${{ secrets.NETLIFY_AUTH_TOKEN }}
    NETLIFY_SITE_ID: ${{ secrets.NETLIFY_SITE_ID }}
    with:
    args: deploy --dir=build --prod
    secrets: '["NETLIFY_AUTH_TOKEN", "NETLIFY_SITE_ID"]'
```
