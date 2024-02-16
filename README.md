# cg-deploy-stratos 

This is part of [cloud.gov](https://cloud.gov/), deployment pipeline for
[Stratos Console](https://github.com/cloudfoundry/stratos).

## Customizing the frontend

### Get dependencies

- Install [NodeJs](https://nodejs.org)
- Install [Angular CLI](https://cli.angular.io/)
  ```
  npm install -g @angular/cli
  ```
- Clone this repository
  ```
  git clone https://github.com/18F/cg-deploy-stratos.git
  ```
- Clone the upstream [Stratos
  project](https://github.com/cloudfoundry/stratos)
  ```
  git clone https://github.com/cloudfoundry/stratos.git
  ```
- Change your working directory to the upstream repository directory
  ```
  cd stratos
  ```
- Link the `custom-src` directory from this repository into the upstream
  repository directory
  ```
  ln -sf ../cg-deploy-stratos/custom-src .
  ```

### Deploy the backend to your cloud.gov sandbox

- Pre-build the assets to run on cloud.gov
  ```
  npm install
  npm run customize
  npm run prebuild-ui
  ```
- Decide on a URL for the backend that we'll deploy in our sandbox,
e.g., `stratos-{myinitials}.app.cloud.gov`.
- Push the app to your cloud.gov sandbox
  ```
  cf target -o sandbox-{org} -s {my.email@address.gov}
  cf push stratos -m 1G -n stratos-{myinitials} -d app.cloud.gov
  ```

### Create a service instance user so you can login

- Create a service instance of the `cloud-gov-service-account` service, called
  `stratos-account`, using the `space-auditor` plan/role
  ```
  cf create-service cloud-gov-service-account space-auditor stratos-account
  ```
- Create a service key tied to that instance
  ```
  cf create-service-key stratos-account stratos-account-creds
  ```
- Get a copy of the credentials so you can login later
  ```
  cf service-key stratos-account stratos-account-creds
  ```

### Modify frontend configuration

- Copy the template for proxy configuration
  ```
  cp proxy.conf.template.js proxy.conf.js
  ```
- Now edit `proxy.conf.js` and change the `host` to the chosen hostname you
  used to deploy the backend. e.g. `stratos-{myinitials}.app.cloud.gov`.

### Run the frontend

- Run `npm start` for a dev server. (the app will automatically reload if
  you change any of the source files)
- Navigate to `https://localhost:4200/`
- Login with the credentials you setup earlier

### Customize

- Follow the customization docs for Stratos, making changes in `custom-src`
  directory
- Once your changes are done, switch over to the directory for this
  repository, and commit your changes to GitHub

## Contributing

See [CONTRIBUTING](CONTRIBUTING.md) for additional information.

## Public domain

This project is in the worldwide [public domain](LICENSE.md). As stated in
[CONTRIBUTING](CONTRIBUTING.md):

> This project is in the public domain within the United States, and
> copyright and related rights in the work worldwide are waived through the
> [CC0 1.0 Universal public domain
> dedication](https://creativecommons.org/publicdomain/zero/1.0/).
>
> All contributions to this project will be released under the CC0
> dedication. By submitting a pull request, you are agreeing to comply with
> this waiver of copyright interest.
