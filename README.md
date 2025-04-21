
Facebook Clone is a real-time socialnetwork application built with `React` and `AWS`. 


## Features
1. Signup and signin authentication
2. Forgot password and reset password
3. Change password when logged in
4. Create, read, update and delete posts with images and gifs
5. Post reactions
6. Comments
7. Followers, following, block and unblock
8. Private chat messaging with text, images, gifs, and reactions
9. Image upload
10. In-app notification and email notification
11. Custom components
12. Custom React hooks
13. Unit tests
14. Redux implementation using redux-toolkit

## Main Tools
- Create react app
- React
- Redux-toolkit
- Axios
- Date-fns
- React redux
- React router DOM
- SocketIO
- React icons
- SASS
- Lodash
- Jest
- React testing library
- ESLint and Prettier
- React app rewired
- React loading skeleton
- Mock service worker

## Requirements

- Node 16.x or higher
- Giphy API key. You can create an account and obtain a key [here](https://developers.giphy.com/)

- You'll need to copy the contents of `.env.develop`, add to `.env` file and update with the necessary information.

## Local Installation

- There are three different branches develop, staging and main. The develop branch is the default branch.

```bash
git clone -b develop https://github.com/GreatDanPeng/Facebook-Clone
cd Facebook-Clone
npm install
```
- To start the server after installation, run
```bash
npm start
```

## Unit tests

- You can run the command `npm run test` to execute the unit tests added to the components.

## Update APP_ENVIRONMENT

- Inside the `axios.js` file found via this path `src/services`, there is a variable called `APP_ENVIRONMENT`.
- If you are setting up the application locally, the variable name needs to be `local`.
- If you are deploying based on the branch, for example develop branch, the variable value needs to be `development`.


## AWS Resources Used
- S3
- Route53
- AWS Certificate Manager
- Cloudfront

## AWS Infrastructure Setup with Terraform
- Install [terraform](https://www.terraform.io/downloads)
- Update the `variables.tf` file with the correct data. Update the properties with comments.
- To store your terraform remote state on AWS, first create a unique S3 bucket with a sub-folder name called `develop`.
- Add that S3 bucket name to `main.tf` file. Also add your region to the file.
- Inside your backend project, make sure to change the `CLIENT_URL` property inside your `.env.develop` file and go through the process of zipping and uploading to s3. After uploading the zip file, you can create the resources.
- Once your backend resources have been created and running, you need to run the terraform apply command to create your frontend resources. But first,
  - if you already followed the instructions for the backend setup, you can reuse the same S3 bucket
  - inside the bucket, create a sub-folder called frontend and inside the folder, create another folder called develop. Bucket path should be something like `<your-s3-bucket>/frontend/develop`
  - if you intend to use a new bucket
    - create a new s3 bucket to store env files
    - inside the created s3 bucket, add a sub-folder called frontend and inside the frontend folder another sub-folder called develop. Bucket path should be something like `<your-s3-bucket>/frontend/develop`
  - you need to upload your `.env` file to the s3 bucket you created for storing env files (No need for zipping). Upload using aws cli
    - `aws --region <your-region> s3 cp .env s3://<your-s3-bucket>/frontend/develop/`
- Make sure to update the `APP_ENVIRONMENT` foind in src/services before deploying.
- Once the upload is complete, you can execute inside the `infrastructure` folder, the commands:
  - `terraform init`
  - `terraform validate`
  - `terraform fmt`
  - `terraform plan`
  - `terraform apply -auto-approve`
- It will take sometime to create the resources. If everything works well, you should be able to access your application.
- To destroy all created resources, run
  - `terraform destroy`

### Before Starting a Build
- Inside the `circleci.yml` file, you need to make some updates.
- There are some properties named `<variable-prefix>` that you need to replace with the `prefix` value from your terraform `variables.tf`. Search the config.yml file and replace `<variable-prefix>`.
- Also, there are some other properties named `<your-s3-bucket>`. Replace it with your s3 bucket name that you created for storing your `.env` file.
