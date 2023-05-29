# Deploy Astro using FTP

This action for [Astro](https://github.com/withastro/astro) builds your Astro project for deployment using traditional FTP upload.

## Usage

> **Note**: Want to get started even faster? Create a repository from our minimal [template](https://github.com/radenpioneer/astro-ftp-template)!

### Inputs

#### Required

> **Warning**: Never put your sensitive information in version control! Put these information in `secrets` variable.

- `server` - The address of your FTP server, e.g `ftp.myftpserver.com`.
- `username` - The username of your FTP server.
- `password` - The password of your FTP server.


#### Optional

- `directory` - The path to upload to, on your FTP server, relative to your FTP root. Must end with trailing slash, e.g `www/`. Defaults to `public_html/`.
- `protocol` - The protocol to use on your FTP server. Accepts `ftp`, `ftps`, and `ftps-legacy`. Defaults to `ftp`.
- `port` - The port of your FTP server. Defaults to `21`.
- `path` - The root location of your Astro project inside the repository.
- `node-version` - The specific version of Node that should be used to build your site. Defaults to `16`.
- `package-manager` - The Node package manager that should be used to install dependencies and build your site. Automatically detected based on your lockfile.
- `resolve-dep-from-path` - If the dependency file should be resolved from the root location of your Astro project. Defaults to `true`.

### Example workflow:

#### Build and Deploy to GitHub Pages

Create a file at `.github/workflows/deploy.yml` with the following content.

```yml
name: Deploy to GitHub Pages

on:
  # Trigger the workflow every time you push to the `main` branch
  # Using a different branch name? Replace `main` with your branch’s name
  push:
    branches: [ main ]
  # Allows you to run this workflow manually from the Actions tab on GitHub.
  workflow_dispatch:
  
# Allow this job to clone the repo and create a deployment
permissions:
  contents: read

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout your repository using git
        uses: actions/checkout@v3
      - name: Install, build, and upload your site output
        uses: radenpioneer/astro-ftp-deploy@v0.1.0
        with:
          server: ${{ secrets.FTP_SERVER }}
          username: ${{ secrets.FTP_USERNAME }}
          password: ${{ secrets.FTP_PASSWORD }}
          # directory: public_html/ # The path to upload to, on your FTP server, relative to your FTP root. Must end with trailing slash. (optional)
          # protocol: ftp # The protocol to use on your FTP server. (optional)
          # port: "21" # The port of your FTP server. (optional)
          # path: . # The root location of your Astro project inside the repository. (optional)
          # node-version: 16 # The specific version of Node that should be used to build your site. Defaults to 16. (optional)
          # package-manager: yarn # The Node package manager that should be used to install dependencies and build your site. Automatically detected based on your lockfile. (optional)
          # resolve-dep-from-path: false # If the dependency file should be resolved from the root location of your Astro project. Defaults to `true`. (optional)

```

### SFTP Support

SFTP is unsupported at current time.

## Attribution

- This Github Action is imported from Astro's official [`withastro/actions`](https://github.com/withastro/action), and modified for FTP deployment.
- This action uses [`SamKirkland/FTP-Deploy-Action`](https://github.com/SamKirkland/FTP-Deploy-Action) under the hood. 
