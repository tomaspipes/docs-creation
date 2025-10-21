# How to contribute

The official documentation format is **Markdown** , with some additional configuration pieces pulled
from Jekyll, to setup the website correctly. See [Front Matter][front-matter].

For complete reference on all UI elements and structure, consult the
[just-the-docs documentation][just-the-docs].

## Building the Site

Setup of the site follows the following guide:
[Testing your Github Pages site locally with Jekyll][setup-github-pages]

It is necessary to have **Ruby**, **Jekyll** and **Bundler** installed.

After installing the pre-requisites, all you need to do is:

```bash
cd docs/
bundler install
bundler exec jekyll serve
```

To preview the site in your browser, navigate to `http://localhost:4000`

## Jekyll Configuration Items

Under the `docs/` directory, you will find the basic configuration items for Jekyll:

```bash
docs/
├── 404.html  # Defines 404 page
├── about.md  # Defines contents of the About section
├── _config.yml  # Global configuration items for the site
├── Gemfile  # Definition of external dependencies, similar to a pip requirements file but for Ruby
├── Gemfile.lock  # Lock file with the pinned versions of the dependencies used
├── index.md  # Defines content for Home/landing page of the site
└── srcs  # Directory containing all our Markdown sources of the documentation
```

Then under the `docs/srcs` directory, this is the currently proposed structure:

```bash
docs/srcs/
└── _example_section  # Defines a new collection/section
    ├── parent_page  # Sections can have groups of pages collected under one parent page
    │   ├── child_page.md
    │   └── index.md
    └── standalone_page_in_section.md  # Sections can have standalone pages
```

Note that if your page has assets, such as image, current convention is that those images are held
in a directory named `assets/` at the same location of the page.

## Documentation structure

There are three elements that define the documentation structure: **pages**, **collections** and
**page hierarchy**.

A **page** is well, just a page. It is represented as a single markdown file and can be contained
within a collection or not.

A **collection** is more commonly refered as a section in the book. It's a top-level structure that
groups pages together.

Pages can define hierarchical relationships between themselves, you can organize, for example, a
group of pages describing christmas recipes under a parent page named "Christmas Recipes" and
collect this parent page in a section called "Holiday Recipes".

### Adding a page

A page is just a markdown page with Front Matter variables. The minimum variables required are
`layout` and `title`. Pages should use the `default` layout.

A minimal page is just a Markdown file with the following Front Matter header:

```markdown
---
layout: default
title: Foo title
---
```

### Collection definition (new section)

To define a new collection, these are the steps:

1. Choose a collection name, e.g. "my_collection". Note that this name is not the one that's
actually going to be displayed on the site.
2. Create a directory within `docs/srcs/` with the name of your collection prefixed by an
underscore, e.g. "\_my_collection/",
3. Add your new collection to the Jekyll configuration `_config.yml`. Add `output: true` to render
all Markdown files under that collection directory

```markdown
collections:
  my_collection:
    output: true  # Link all files inside collection
```

4. Add your collection to the just-the-docs Jekyll configuration `_config.yml`

```markdown
just_the_docs:
  collections:
    my_collection:
      name: Greatest section ever
      nav_fold: true  # This will fold the contents by default when rendering the section
```

And voilá! New section is now added.

### Defining hierarchy between two pages

A parent page can be created either on the root of the documentation or within a section (with
preference for the latter). For this example we'll create a parent page and child page within a
section.

To create a parent page in a section with a child page:

1. Create a directory within a collection, let's reuse the previous "my_collection" section.
2. In that directory, add a `index.md` file and a 'child_page.md` file.

```bash
docs/srcs/_my_collection/
├── parent_page
│   ├── child_page.md
│   └── index.md

```

3. On the `index.md` file representing the parent page, add the following
Front Matter to mark it as a parent page, in particular `has_children`:

```markdown
---
layout: default
title: Example Parent Page
has_children: true
---
```

4. On the child pages, add the following Front Matter to link it to the parent page. The `parent`
value must have the `title` value of the parent page:

```markdown
---
layout: default
title: example sub-page
parent: Example Parent Page
---
```

And done!

### Defining hierarchy for double nested pages

For pages with two levels of nesting, from child to parent to grand-parent, the same logic
applies with the caveat that the grand-parent page must also be marked in the child page.

Taking the previous example, if we wanted to create another page within, we'd have to first repeat
1 to 3 within the original parent page then, on the last step, instead of only adding the parent
page (here called `Example sub-Parent Page`) we would also have to add the grand-parent.

```markdown
---
layout: default
title: example sub-sub-page
parent: Example sub-Parent Page
grand_parent: Example Parent Page
---
```

- Add guidelines regarding image formats (SVG strongly preferred)

[front-matter]: https://jekyllrb.com/docs/front-matter/
[just-the-docs]: https://just-the-docs.com/
[setup-github-pages]: https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll
