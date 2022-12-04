# Sage 10 with Gutenberg Full Site Editing (FSE)

## Requirements

- Gutenberg Plugin is not required, but adds extra Gutenberg-specific functionality that is missed otherwise in WordPress core.
- A [patched `roots/acorn` package](https://github.com/roots/acorn/pull/141) is used by the theme as theme runtime dependency (in theme [`composer.json`](https://github.com/strarsis/sage10-fse/blob/master/composer.json#L43-L49)) which loader preserves the block template paths.

## Theme installation

1. Clone this repository.
2. Invoke `composer install`.
3. (Optional) Ensure suitable `node` version, e.g. using `nvm` from the `.nvmrc` by invoking `nvm install && nvm use`.
4. Invoke `npm install`.
5. Invoke `npm run build`.
6. (Mount the theme into a WordPress site and activate it).
7. Open the Site Editor:
  - Admin area → Appearance → Edit (beta)
  - (Logged in as editor+) → Front end → Admin bar → Edit Site

## Theme structure

```sh
themes/your-theme-name/   # → Root of your Sage based theme
├── templates/            # → Block templates (required for a FSE theme) (formerly named `block-templates`)
│   ├── page.html         # → Block template for singular page
│   ├── index.html        # → Block template for the posts (fallback)
│   ├── home.html         # → Block template for posts page (specific page selected as blog page)
│   └── front-page.html   # → Block template for front page (specific page selected as front page)
├── parts/                # → Block parts (can be used in block templates, among others) (formerly named `block-parts`)
│   ├── header.html       # → Block part for a header (there can be more headers, if needed)
│   └── footer.html       # → Block part for a footer (there can be more footers, if needed)
├── patterns/             # → Block patterns
│   └── some-pattern.php  # → Block example pattern
├── theme.json            # → Generated by Sage theme build process (`bud`) or directly edited (required for a FSE theme)
```

## Notes

- `add_theme_support('block-templates');` is not needed when theme is already autodetected to be a FSE theme (by presence of `theme.json` and `templates/`).
- Disabling FSE using `remove_theme_support('block-templates')` is ignored when block templates are in place.

## Known issues and fixes

### Template part blocks can't render; `Cannot read properties of undefined (reading 'tinymce'`

Block template or block parts contain plain, "naked" HTML without block-specific comments.

#### Related issues

- <https://core.trac.wordpress.org/ticket/55043>
- <https://github.com/bobbingwide/sb/issues/6>

## Customization by user
By default the user can override the theme-provided block templates and block parts, those modifications are stored as special posts in database.
The site editor sidebar can be opened by clicking on the logo/icon on the upper left corner in the side editor.
From that sidebar the block templates and template parts lists can be viewed and the user customizations reset.

## Home versus Front page templates
This was a gotcha for me, as I first hadn't completely understood the exact difference between those two.
This very well made article explains the differences:
https://davidsutoyo.com/articles/difference-front-page-php-home-php/
