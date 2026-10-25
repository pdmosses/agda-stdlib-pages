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

### Create a branch for version `V`

The webpages for each version `V` of the website are to be generated separately,
using a fresh branch **`V-pages`** corresponding to the `V` version of the library.

```sh
git checkout V
git checkout -b V-pages
```

### Copy the [pages] directory to the `V-pages` branch

For example, clone this repository and copy the directory locally.

### Add the [agda-pages repository] as a submodule

```sh
git submodule add https://github.com/pdmosses/agda-pages/ pages/agda-pages
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
    │   ├─ About.md
    │   └─ .nav.yml
    ├─  Makefile
    └─  properdocs.yml
```

Website generation and deployment adds further files to the [pages] directory.

> [!CAUTION]
> By default, non-Markdown files in the `pages/docs` directory are included
> when the generated website is deployed.

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
make properdocs
make index
make check
make web
make serve
```

-   `make properdocs` sets the `site_name`, `repo_name`, and `repo_url` all 
    to refer to version `V`.

-   `make index` generates the source file for the library index, `index.agda`.
    This involves a call of `cabal`.

-   `make check` loads all the Agda source files for the library. This may take
    more than 5 minutes the first time; subsequent calls take about 15 seconds.

-   `make web` uses Agda to generate highlighted HTML from each source file,
    and generates Markdown sources for webpages in `pages/docs/`. This takes
    about one minute.

-   `make serve` creates a local preview of the generated website at
    `http://localhost:SERVER`, where the port number `SERVER` is as set in the
    `Makefile` (default: `8000`). This takes about one minute.
    
The preview does *not* include previously deployed versions of the website,
nor do the pages display a version selector.

## Website deployment to GitHub Pages


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
