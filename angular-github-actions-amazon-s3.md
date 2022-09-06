1. Create bucket on aws
2. create  IAM on aws (with amazonFullAccess) 
3. go  to github settings/secrets/dependatbot and create new repository secret
  `AWS_ACCESS_KEY_ID
 AWS_SECRET_ACCESS_KEY`
4. create angular application
5. add package.json
 `  "build:prod": "ng build --configuration production",
  "test:headless": "ng test --watch=false --browsers=ChromeHeadless"`
6. .github/workflows/gh-pages.yml create and add 
  `name: GitHub Pages

on:
  push:
    branches:
    - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Configure AWS Credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: sa-east-1

    - name: Checkout
      uses: actions/checkout@v2

    - name: Setup Node.js
      uses: actions/setup-node@v2
      with:
        node-version: 16

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm run test:headless

    - name: Build
      run: npm run build:prod

    - name: Deploy
      if: success()
      run: aws s3 sync ./dist/projectname s3://buccket_name`  
