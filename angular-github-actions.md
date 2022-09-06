1.  create the repository on github
2.  Create the Angular application
3.  in package.json add script like
  `"build:prod": "ng build --configuration production --base-href /repo_name/",
  "test:headless": "ng test --watch=false --browsers=ChromeHeadless"`
4.  Create the .github/workflows/gh-pages.yml and add code
    `name: GitHub Pages

on:
  push:
    branches:
    - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout
      uses: actions/checkout@v2

    - name: Setup Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '16'

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm run test:headless

    - name: Build
      run: npm run build:prod

    - name: Deploy
      if: success()
      uses: peaceiris/actions-gh-pages@v3
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        publish_dir: dist/projectname
        enable_jekyll: true`
