# Agda Standard Library Pages

This repository supports generation of a prototype website listing the
`master` branch and previous releases of the Agda standard library.

It uses **[Agda-Pages]** to generate a website with **module navigation**
between **highlighted, hyperlinked listings** of Agda code. When deployed,
the website displays a **version selector** on all pages. See the
**[Agda-Pages About]** page for an overview of further features of generated
websites.

> [!IMPORTANT]
> Below, **`V`** stands for **`master`** or a version tag **`vX.Y`** of the library.

## Installation

The installation commands are to be run from the **project root** directory.

### Prerequisites

-   A clone/fork of the [agda-stdlib repository]
-   Executables of all the dependencies of [Agda-Pages]

> [!IMPORTANT]
> The dependency [sd] has a breaking change in relation to v1.0.0.
> If `sd` v1.1.0` is installed, set `SD = sd -A` in your `Makefile`.

### Create a branch for version `V`

The webpages for each version `V` of the website are to be generated separately,
using a fresh branch **`V-pages`** corresponding to the `V` version of the library.

```sh
git switch V --detach
git switch -c V-pages
```

Publish the `V-pages` branch on GitHub.

### Copy the [pages] directory to the `V-pages` branch

For example, clone this repository and copy the pages directory locally:

```sh
cp -r PATH-TO-CLONE/agda-stdlib-pages/pages .
```

### Add the [agda-pages repository] as a submodule

```sh
git submodule add --force https://github.com/pdmosses/agda-pages/ pages/agda-pages
echo "\tbranch = main" >> .gitmodules
git push
```

> [!NOTE]
> Appending `branch = main` to `.gitmodules` avoids potential confusion
> when Git is looking for the default branch.

The root directory of the `V-pages` branch should now include:

```
├─  ...
└─  pages/
    ├─  agda-pages/
    │   └─ ...
    ├─  docs/
    │   ├─ .nav.yml
    │   └─ About.md
    ├─  Makefile
    └─  properdocs.yml
```

Website generation and deployment add further files to the [pages] directory.

> [!CAUTION]
> By default, any non-Markdown files added to the `pages/docs` directory are
> *included* when the generated website is deployed.

To update the `V-pages` branch to the latest changes to the [agda-pages repository]:

```sh
git submodule update --remote
```

If the `V-pages` branch is subsequently cloned, the submodule in the clone also
has to be *initialised*:

```sh
git submodule update --init --recursive
```

## Website generation

```sh
cd pages
make properdocs # instead of make check
make copy-html  # instead of make gen-html
make gen-pages  # instead of make web
make serve
```

-   `make properdocs` sets the `site_name`, `repo_name`, and `repo_url` values 
    in `pages/properdocs.yml`.

-   `make copy-html` copies the HTML files for version `V` from the `gh-pages`
    branch of the upstream [agda-stdlib repository] to `pages/html/`.

-   `make gen-pages` generates Markdown source files for webpages in
    `pages/docs/` from the HTML files in `pages/html`.

-   `make serve` creates a local preview of the generated website at
    `http://localhost:SERVER/agda-stdlib/`, where the port number `SERVER` is
    as set in the `Makefile` (default: `8000`).
    
The preview does *not* include previously deployed versions of the website,
nor do the pages display a version selector.

## Website deployment to GitHub Pages

The generated website for each version of the library is deployed to the
`pages` directory using the `gh-pages` branch.

> [!WARNING]
> If you push to the `master`, `experimental`, or any `v*-release` branch
> of your fork, GitHub runs the actions in `.github/worflows/ci-ubuntu.yml`.
> That overwrites the `gh-pages` branch in your fork and **removes** the
> `pages` directory!

### Website initialisation

The website needs to be initialised before the first version is deployed.

> [!CAUTION]
> Initialisation **removes** all previously-deployed versions!

```sh
cd pages
make start-versioning
```

### Version deployment

Deploy the generated website as version `V`:

```sh
cd pages
make deploy VERSION=V
```

The URL of the index page of the deployed version `V` is
`https://OWNER.github.io/agda-stdlib/pages/V/`.

> [!TIP]
> If the value of `VERSION` is set to `V` in the [Makefile], the `VERSION=V`
> argument can be omitted in the `make` commands.

### Default version

Set a **previously-deployed** version `V` as the `default` version of the
generated website:

```sh
cd pages
make default VERSION=V
```

This deploys `default` as an alias for version `V`. It may take several minutes.

It also redirects the URL `https://OWNER.github.io/agda-stdlib/pages/` of the
website home page to the index page of the `default` version
`https://OWNER.github.io/agda-stdlib/pages/default/`.

### Website version updates

After pulling changes to the `master` branch of a fork of the
[agda-stdlib repository]:

-   switch to the `master-pages` branch and push the changes
-   [regenerate] and [redeploy] the website for the `master` version

(It is *not* necessary to reset the `default` version.)

Versions generated and deployed using previous releases of the present
repository may be updated in the same way.

[pages]:                  pages/
[agda-pages submodule]:   pages/agda-pages/
[docs]:                   pages/docs/
[About.md]:               pages/docs/About.md
[.nav.yml]:               pages/docs/.nav.yml
[Makefile]:               pages/Makefile
[properdocs.yml]:         pages/properdocs.yml

[regenerate]:             #website-generation
[redeploy]:               #version-deployment

[Agda-Pages]:             https://pdmosses.github.io/agda-pages/default/
[Agda-Pages About]:       https://pdmosses.github.io/agda-pages/default/About/
[agda-pages repository]:  https://github.com/pdmosses/agda-pages/
[agda-stdlib repository]: https://github.com/agda/agda-stdlib/
