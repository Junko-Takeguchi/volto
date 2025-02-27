---
myst:
  html_meta:
    "description": "Installing Volto"
    "property=og:description": "Installing Volto"
    "property=og:title": "Getting Started"
    "keywords": "Volto, Plone, frontend, React, install, nvm, Node.js , JavaScript"
---

(frontend-getting-started-label)=

# Getting Started

```{warning}
This chapter of the documentation has been superseded by the official Plone 6 Documentation chapter {ref}`install-index-label`.
This chapter contains some legacy information that may be useful to Plone 5.2 development.
```


(frontend-getting-started-installing-volto-label)=

## Installing Volto

Volto can be installed in any operating system assuming that the following pre-requisites are met:

- [Node.js LTS (18.x)](https://nodejs.org/en)
- [Python](https://www.python.org/) - See below for specific versions.
- [Docker](https://www.docker.com/get-started) (if using the Plone docker images)

The versions of Python that are supported in Volto depend on the version of Plone that you use.

| Plone | Python | Volto |
|---|---|---|
| 5.2 | 2.7, 3.6-3.8 | 15.0 |
| 6.0 | 3.8-3.10 | 16.0 |

Depending on the operating system that you are using, some of the following pre-requisites might change.
They assume you have a macOS/Linux machine.


(frontend-getting-started-components-processes-running-label)=

## Components / Processes running

There are three processes continuously running when you have a working Volto website:

1. A frontend web application running in your browser (JavaScript)
2. A {term}`Node.js` server process that delivers the JavaScript to the client and does
   {term}`server-side rendering` (SSR) of your pages on first request (JavaScript, the
   Razzle package is used for SSR)
3. A Plone server process that stores and delivers all content through a REST API (Python)

When you start with Volto most of the first customisations you will want to make (or maybe
ever need to make) are in the JavaScript code used in the browser and Razzle process. Therefore
this getting started chapter will focus on installing a Node.js/JavaScript environment locally
and suggest you start the API backend using a container.


(frontend-getting-started-install-nvm-label)=

## Install nvm (Node.js version manager)

If you have a working Node.js JavaScript development already set up on your machine or you prefer
another management tool to install/maintain Node.js this step is not needed. If you have less
experience with setting up JavaScript, it's a good idea to integrate nvm for development, as
it provides easy access to any Node.js released version.

1.  Open a terminal console and type:

    ```bash
    touch ~/.bash_profile
    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.39.1/install.sh | bash
    ```

    (Please check the latest available version of nvm on the [main README](https://github.com/nvm-sh/nvm)

2.  Close the terminal and open a new one or execute:

    ```bash
    source ~/.bash_profile
    ```

3.  Test it:

    ```bash
    nvm version
    ```

4.  Install any active LTS version of Node.js (https://github.com/nodejs/release#release-schedule):

    ```bash
    nvm install 18
    nvm use 18
    ```

5.  Test Node.js :

    ```bash
    node -v
    ```

    ```{note}
    If you're using the fish shell, you can use [nvm.fish](https://github.com/jorgebucaran/nvm.fish)
    ```

    ```{note}
    Volto supports currently active Node.js LTS versions based on [Node.js
    Releases page](https://github.com/nodejs/release#release-schedule), starting with Node.js 16 LTS.
    ```


(frontend-getting-started-yeoman-label)=

## Yeoman

Install {term}`Yeoman`, a scaffolding tool.

```shell
npm install -g yo
```


(frontend-getting-started-yarn-label)=

## Yarn (Node.js package manager)

Install the Yarn Classic version (not the 2.x one!), of the popular Node.js package manager.

1. Open a terminal and type:

    ```bash
    curl -o- -L https://yarnpkg.com/install.sh | bash
    ```

2. Test it, running:

    ```bash
    yarn -v
    ```

    ```{tip}
    As alternative, you can install `yarn` using several approaches too, depending on the
    platform you are on. Take a look at the original `yarn`
    [documentation](https://classic.yarnpkg.com/lang/en/) for a list of them.
    ```


(frontend-getting-started-use-or-install-docker-label)=

## Use or Install Docker

In order to run the API backend, it's recommended to start run it in a container.
For this getting started section we assume you are either using Linux, or Mac. Most
modern Linux distributions have docker in their package manager available.

To install Docker desktop for Mac, here are the detailed instructions:

    https://hub.docker.com/editions/community/docker-ce-desktop-mac

1. Download the appropriate .dmg for your Intel or Apple chip.

2. Install the package as any other Mac software, if required, follow
   instructions from:

    https://docs.docker.com/desktop/install/mac-install/

3. Check that docker is installed correctly, open a new terminal and type:

    ```shell
    docker ps
    ```

    should not throw an error and show the current running containers.


(frontend-getting-started-run-a-volto-ready-plone-docker-container-label)=

## Run a Volto ready Plone Docker container

When you have installed Docker, you can use the official Plone Docker container with the proper configuration for Volto using the `plone.volto` add'on right away by issuing:

```shell
docker run -it --rm --name=plone \
  -p 8080:8080 -e SITE=Plone -e \
  ADDONS="plone.restapi==8.18.0 plone.app.iterate==4.0.2 plone.rest==2.0.0a1 plone.app.vocabularies==4.3.0 plone.volto==3.1.0a8" \
  -e PROFILES="plone.volto:default-homepage" \
  plone/plone-backend
```

```{tip}
This setup is meant only for demonstration and quick testing purposes, since it destroys the container on exit (`--rm`).
In case you need production-ready deployment, check the latest [Plone Deployment Training](https://training.plone.org/plone-deployment/index.html).
```

```{note}
The example above does not persist yet any changes you make through Volto in
the Plone docker container backend! For this you need to map the /data directory
in the container properly. Check Docker
[storage documentation](https://docs.docker.com/storage/) for more information.

As a quick example: if you add
`--mount type=bind,source="$(pwd)/plone-data",target=/data`
to the previous example. The local subdirectory plone-data relative to where you
execute `docker run` will be use to persist the backend server data.
```

If you are somewhat familiar with Python development, you can also install Plone locally
without using Docker. Check the [backend configuration](../configuration/backend.md) section.
It also has more information on `plone.volto`.


(frontend-getting-started-install-volto-label)=

## Install Volto

Use the project generator helper utility.
The latest stable release of Volto will be installed by default.
You may choose to install the canary version, which is the latest alpha release, using the `--canary` flag.

1.  Open a terminal and execute:

    ```shell
    npm install -g yo @plone/generator-volto
    # install latest stable release
    yo @plone/volto
    # or install latest alpha release
    yo @plone/volto --canary
    ```

    See {doc}`../recipes/creating-project` for more advanced options that can be passed to the generator.

2.  Answer the questions when prompted, and provide the name of the new app (folder) to be created.
    For the sake of this documentation, provide `myvoltoproject` as the project name.

    ````{note}
    You can run the generator with parameters to tailor your requirements.

    ```shell
    yo @plone/volto --help
    ```

    or take a look at the [README](https://github.com/plone/volto/blob/main/packages/generator-volto/README.md) for more information.
    ````

3.  Change directory to the newly created folder `myvoltoapp` (or the one you've chosen):
    ```bash
    cd myvoltoapp
    ```

    Then start Volto with:

    ```bash
    yarn start
    ```

    This command will build an in-memory bundle and execute Volto in development mode.
    Open a browser to take a look at http://localhost:3000

    ```{danger}
    `create-volto-app` was deprecated from January 2021, in favor of [@plone/generator-volto](https://github.com/plone/generator-volto).
    ```


(frontend-getting-started-build-the-production-bundle-label)=

## Build the production bundle

In production environments, you should build an static version of your (Volto) app. The
app should be run in a Node.js process (because of the {term}`server-side rendering`
part), but it also have a client part that is provided and deployed by the server
side rendering process.

1.  Compile the app using the command:

    ```bash
    yarn build
    ```
    The resultant build is available in the `build` folder.

2.  Run the Volto Node.js process
    ```bash
    yarn start:prod
    ```

    to run the Node.js process with the production build. You can also run it manually:

    ```bash
    NODE_ENV=production node build/server.js
    ```
    Your production ready Volto will be available in http://localhost:3000
