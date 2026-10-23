# Agda Standard Library Pages

This repository supports generation of a website listing the
`master` branch and previous releases of the Agda standard library.

It uses **[Agda-Pages]** to generate a website with **module navigation**
between **highlighted, hyperlinked listings** of Agda code. When deployed,
the website displays a **version selector** on all pages.

See the **[Agda-Pages About]** page for an overview of the features
of the generated website.

> [!WARNING]
> Below, `V` stands for `master` or a release tag `vX.Y`.

## Installation

The installation commands are to be run from the **project root** directory.

### Prerequisites

-   A clone/fork of the [Agda StdLib repository]
-   Executables of all the dependencies listed in the [Agda-Pages README]

### Create a branch for version `V`

The webpages for each version `V` of the website are generated separately,
using a fresh branch `V-pages`.

```sh
git checkout V
git checkout -b V-pages
```

### Add this repository as a submodule

```sh
git submodule add https://github.com/pdmosses/agda-stdlib-pages/
echo "\tbranch = main" >> .gitmodules
git push
```

> [!NOTE]
> Appending `branch = main` to `.gitmodules` avoids potential confusion
> that can arise from Git sometimes looking for a `master` branch.

The root directory should now include `agda-stdlib-pages`:

```
├─  ...
└─  agda-stdlib-pages/
    ├─  LICENSE
    ├─  README.md
    └─  pages/
        ├─  agda-pages/
        │   └─ ...
        ├─  docs/
        │   ├─ About.md
        │   └─ .nav.yml
        ├─  Makefile
        └─  properdocs.yml
```

The [pages directory] includes:

-   a submodule `agda-pages`, referring to the [Agda-Pages repository]
-   a Markdown source file [docs/About.md]
-   a configuration file [docs/.nav.yml] for website navigation
-   a [Makefile] for generating and deploying a version of the website
-   a configuration file [properdocs.yml] for website deployment

To update the `V-pages` branch to the latest changes to the submodule repository:

```sh
git submodule update --remote
```

## Website generation

```sh
cd agda-stdlib-pages/pages
make index
make check
make web
make serve
```

A local preview of the generated website is served at `http://localhost:SERVER`,
where the port number `SERVER` can be set in the `Makefile` (default: `8000`).

The local preview does *not* include previously deployed versions of the website,
nor do the pages display a version selector.

## Website deployment to GitHub Pages

### Website initialisation

> [!CAUTION]
> Initialisation **removes** all previously-deployed versions!

```sh
cd agda-stdlib-pages/pages
make start-versioning
```

### Version deployment

> [!CAUTION]
> If the value of `VERSION` is set using `=` or `:=` in the [Makefile],
> the `make` commands below **ignore** the `VERSION=V` argument!

Deploy the generated website as version `V`:

```sh
cd agda-stdlib-pages/pages
make deploy VERSION=V
```

The URL of the index page of the deployed version `V` is
`https://USER.github.io/agda-stdlib/pages/V/`, where `USER` is your GitHub
user name.

### Default version

To set a previously-deployed version `V` as the `default` version of the
generated website:

```sh
cd agda-stdlib-pages/pages
make default VERSION=V
```

This deploys `default` as an alias for version `V`. It also redirects the 
URL `https://USER.github.io/agda-stdlib/pages/` of the website home page
to the index page of version `V`.

> [!TIP]
> Setting the `default` version may take several minutes.

### Website version updates

After pulling changes to the `master` branch of a fork of the
[Agda StdLib repository]:

-   switch to the `master-pages` branch and push the changes
-   [regenerate] and [redeploy] the website for the `master` version

(It is *not* necessary to reset the `default` version.)

Versions generated and deployed using previous releases of the present
repository may be updated in the same way.

[regenerate]: #website-generation
[redeploy]:   #version-deployment

[Agda-Pages]:            https://pdmosses.github.io/agda-pages/default/
[Agda-Pages About]:      https://pdmosses.github.io/agda-pages/default/About/
[Agda-Pages User Guide]: https://pdmosses.github.io/agda-pages/default/User-Guide/
[Agda-Pages repository]: https://github.com/pdmosses/agda-pages/
[Agda-Pages README]:     https://github.com/pdmosses/agda-pages/blob/main/README.md
[linkcheck]:             https://github.com/filiph/linkcheck/

[Agda StdLib repository]: https://github.com/agda/agda-stdlib/
[Agda StdLib website]:    https://agda.github.io/agda-stdlib/master/
