# Agda-StdLib Pages

This website illustrates and tests use of **[Agda-Pages]** to
**generate websites** with **module navigation** between
**highlighted, hyperlinked listings** of Agda code.

## Website

The **[README]** section was generated from plain Agda files in the
[doc directory].

The **[Library]** section was generated from plain Agda files created by
[GenerateEverything.hs]. The Library home page is also the website home page.

The generated website displays a **version selector** on all pages.

See the **[Agda-Pages About]** page for an overview of the features
of the generated website, and for links to further examples.

## Generation

The **[Agda-Pages User Guide]** explains how to generate a website listing
Agda code in any GitHub repository.

See the **[Agda-Pages README]** for how to install Agda-Pages,
and for a list of its main software dependencies.

Running the following shell commands in the [pages directory] generated this
website from the [repository]:

```shell
make everything
make check
make web
make serve
```

The generated website was deployed to GitHub Pages at
`https://pdmosses.github.io/agda-stdlib/pages/.../` by the command:

```shell
make deploy VERSION=...
```

[README]:     README.md
[Library]:    index.md
[Agda-Pages About]:      https://pdmosses.github.io/agda-pages/About/
[Agda-Pages User Guide]: https://pdmosses.github.io/agda-pages/User-Guide/
[Agda-Pages README]:     https://github.com/pdmosses/agda-pages/blob/main/README.md
[repository]:            https://github.com/pdmosses/agda-stdlib/
[doc directory]:         https://github.com/pdmosses/agda-stdlib/tree/main/doc
[pages directory]:       https://github.com/pdmosses/agda-stdlib/tree/main/pages
[GenerateEverything.hs]: https://github.com/pdmosses/agda-stdlib/blob/main/GenerateEverything.hs
