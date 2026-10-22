# Agda Standard Library Pages

This repository facilitates use of **[Agda-Pages]** to generate
a **versioned website** with **module navigation** between
**highlighted, hyperlinked listings** of Agda code.

## Website

The **[README]** section was generated from the Agda files in the
[doc directory].

The **[Library]** section was generated from the Agda files created by
[GenerateEverything.hs], which import modules from the [src directory].
The Library home page is also the website home page.

When deployed, the website displays a **version selector** on all pages.
It was generated from a [fork] of the [Agda StdLib repository].

> [!WARNING]
> The Agda code in the `master` version of the standard library may have
> been updated since the `master` version of the present website was
> generated. For the definitive listings of the `master` branch, see the
> official [Agda StdLib website].

See the **[Agda-Pages About]** page for an overview of the features
of the generated website, and for links to further examples.

## Installation

This repository supports generation of a versioned website listing the
`master` branch and previous releases[^releases] of the Agda standard library.

[^releases]:
    The supported releases are currently from `v1.7` to `v2.4`.

### Prerequisites

-   A clone/fork of the [Agda StdLib repository]
-   Executables of all the dependencies listed in the [Agda-Pages README]

> [!NOTE]
> All commands below are to be run from the root directory.
>
> Replace version `V` by `master` or by a release tag `vX.Y`.

### Create a branch for `version`

```sh
git checkout V
git checkout -b V-pages
```

### Add this repository as a submodule

```sh
git submodule add https://github.com/pdmosses/agda-stdlib-pages/
```

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

## Website generation

```sh
cd agda-stdlib-pages/pages
make index
make check
make web
make serve
```

A local preview of the generated website is served at `http://localhost:...`,
where the port number is configured by setting `SERVER` in the `Makefile`.

The local preview does not include previously deployed versions of the website,
nor do the pages display a version selector.

### Website deployment to GitHub Pages

Initialise the website:

```sh
cd agda-stdlib-pages/pages
make start-versioning
```

> [!CAUTION]
> Before running the `make` commands below, set the value of `VERSION`
> in the [Makefile].

Deploy the generated website as `VERSION`:

```sh
cd agda-stdlib-pages/pages
make deploy VERSION=V
```

The URL of the index page of the deployed version `V` is
`https://USER.github.io/agda-stdlib/pages/V/`,
where `USER` is your GitHub user name.

Deploy the `default` version of the generated website as version `V`:

```sh
cd agda-stdlib-pages/pages
make default VERSION=V
```

This deploys `default` as an alias for version `V`. It also redirects the 
URL `https://USER.github.io/agda-stdlib/pages/` of the website home page
to the home page of version `V`.

[Library]:               index.md
[README]:                README/index.md

[Agda-Pages]:            https://pdmosses.github.io/agda-pages/default/
[Agda-Pages About]:      https://pdmosses.github.io/agda-pages/default/About/
[Agda-Pages User Guide]: https://pdmosses.github.io/agda-pages/default/User-Guide/
[Agda-Pages repository]: https://github.com/pdmosses/agda-pages/
[Agda-Pages README]:     https://github.com/pdmosses/agda-pages/blob/main/README.md
[linkcheck]:             https://github.com/filiph/linkcheck/

[fork]:                  https://github.com/pdmosses/agda-stdlib/
[add-pages branch]:      https://github.com/pdmosses/agda-stdlib/tree/add-pages
[doc directory]:         https://github.com/pdmosses/agda-stdlib/tree/add-pages/doc
[src directory]:         https://github.com/pdmosses/agda-stdlib/tree/add-pages/src
[pages directory]:       https://github.com/pdmosses/agda-stdlib/tree/add-pages/pages
[docs/.nav.yml]:         https://github.com/pdmosses/agda-stdlib/blob/add-pages/pages/docs/.nav.yml
[docs/About.md]:         https://github.com/pdmosses/agda-stdlib/blob/add-pages/pages/docs/About.md
[Makefile]:              https://github.com/pdmosses/agda-stdlib/blob/add-pages/pages/Makefile
[properdocs.yml]:        https://github.com/pdmosses/agda-stdlib/blob/add-pages/pages/properdocs.yml
[GenerateEverything.hs]: https://github.com/pdmosses/agda-stdlib/blob/add-pages/GenerateEverything.hs

[Agda StdLib repository]: https://github.com/agda/agda-stdlib/
[Agda StdLib website]:    https://agda.github.io/agda-stdlib/master/
