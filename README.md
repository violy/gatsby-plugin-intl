# gatsby-plugin-react-intl

> If can, try theme [gatsby-theme-i18n](https://github.com/gatsbyjs/themes/tree/master/packages/gatsby-theme-i18n), this plugin will no longer be actively maintained

> `gatsby-plugin-react-intl` has supported gatsby v3! Please upgrade `gatsby-plugin-react-intl` to `^3.0.0` to use it

> For gatsby v2, please use `gatsby-plugin-react-intl@1.3.0`

Internationalize your Gatsby site.

Fork from [https://github.com/wiziple/gatsby-plugin-intl](https://github.com/wiziple/gatsby-plugin-intl)

Here are added features:

- `ignoredPaths`: paths that you don't want to genereate locale pages, example: ["/dashboard/","/test/**"], string format is from micromatch https://github.com/micromatch/micromatch
- `redirectDefaultLanguageToRoot`: option for use / as defaultLangauge root path. if your defaultLanguage is `ko`, when `redirectDefaultLanguageToRoot` is true, then it will not generate `/ko/xxx` pages, instead of `/xxx`
- `fallbackLanguage`: option to fallback to the defined language instead of the `defaultLanguage` if the user langauge is not in the list

The other feature just like [https://github.com/wiziple/gatsby-plugin-intl](https://github.com/wiziple/gatsby-plugin-intl)

## Features

- Turn your gatsby site into an internationalization-framework out of the box powered by [react-intl](https://github.com/yahoo/react-intl).

- Support automatic redirection based on the user's preferred language in browser provided by [browser-lang](https://github.com/wiziple/browser-lang).

- Support multi-language url routes in a single page component. This means you don't have to create separate pages such as `pages/en/index.js` or `pages/ko/index.js`.

- Support ignore paths that you don't need to generate locale pages.

## Why?

When you build multilingual sites, Google recommends using different URLs for each language version of a page rather than using cookies or browser settings to adjust the content language on the page. [(read more)](https://support.google.com/webmasters/answer/182192?hl=en&ref_topic=2370587)

## Starters

Demo: [http://gatsby-starter-default-intl.netlify.com](http://gatsby-starter-default-intl.netlify.com)

Source: [https://github.com/wiziple/gatsby-plugin-intl/tree/master/examples/gatsby-starter-default-intl](https://github.com/wiziple/gatsby-plugin-intl/tree/master/examples/gatsby-starter-default-intl)

## Showcase

- [https://picpick.app](https://picpick.app)
- [https://www.krashna.nl](https://www.krashna.nl) [(Source)](https://github.com/krashnamusika/krashna-site)
- [https://vaktija.eu](https://vaktija.eu)
- [https://anhek.dev](https://anhek.dev) [(Source)](https://github.com/anhek/anhek-portfolio)
- [https://pkhctech.ineo.vn](https://pkhctech.ineo.vn) [(Source)](https://github.com/hoangbaovu/gatsby-pkhctech)

_Feel free to send us PR to add your project._

## How to use

### Install package

`npm install --save gatsby-plugin-react-intl`

### Add a plugin to your gatsby-config.js

```javascript
// In your gatsby-config.js
plugins: [
  {
    resolve: `gatsby-plugin-react-intl`,
    options: {
      // language JSON resource path
      path: `${__dirname}/src/intl`,
      // supported language
      languages: [`en`, `ko`, `de`],
      // language file path
      defaultLanguage: `ko`,
      // option to redirect to `/ko` when connecting `/`
      redirect: true,
      // option for use / as defaultLangauge root path. if your defaultLanguage is `ko`, when `redirectDefaultLanguageToRoot` is true, then it will not generate `/ko/xxx` pages, instead of `/xxx`
      redirectDefaultLanguageToRoot: false,
      // paths that you don't want to genereate locale pages, example: ["/dashboard/","/test/**"], string format is from micromatch https://github.com/micromatch/micromatch
      ignoredPaths: [],
      // option to fallback to the defined language instead of the `defaultLanguage` if the user langauge is not in the list
      fallbackLanguage: `en`,
    },
  },
]
```

### You'll also need to add language JSON resources to the project.

For example,

| language resource file                                                                                                              | language |
| ----------------------------------------------------------------------------------------------------------------------------------- | -------- |
| [src/intl/en.json](https://github.com/wiziple/gatsby-plugin-intl/blob/master/examples/gatsby-starter-default-intl/src/intl/en.json) | English  |
| [src/intl/ko.json](https://github.com/wiziple/gatsby-plugin-intl/blob/master/examples/gatsby-starter-default-intl/src/intl/ko.json) | Korean   |
| [src/intl/de.json](https://github.com/wiziple/gatsby-plugin-intl/blob/master/examples/gatsby-starter-default-intl/src/intl/de.json) | German   |

### Change your components

You can use `injectIntl` HOC on any react components including page components.

```jsx
import React from "react"
import { injectIntl, Link, FormattedMessage } from "gatsby-plugin-react-intl"

const IndexPage = ({ intl }) => {
  return (
    <Layout>
      <SEO title={intl.formatMessage({ id: "title" })} />
      <Link to="/page-2/">
        {intl.formatMessage({ id: "go_page2" })}
        {/* OR <FormattedMessage id="go_page2" /> */}
      </Link>
    </Layout>
  )
}
export default injectIntl(IndexPage)
```

Or you can use the new `useIntl` hook.

```jsx
import React from "react"
import { useIntl, Link, FormattedMessage } from "gatsby-plugin-react-intl"

const IndexPage = () => {
  const intl = useIntl()
  return (
    <Layout>
      <SEO title={intl.formatMessage({ id: "title" })} />
      <Link to="/page-2/">
        {intl.formatMessage({ id: "go_page2" })}
        {/* OR <FormattedMessage id="go_page2" /> */}
      </Link>
    </Layout>
  )
}
export default IndexPage
```

## How It Works

Let's say you have two pages (`index.js` and `page-2.js`) in your `pages` directory. The plugin will create static pages for every language.

| file                | English        | Korean         | German         | Default\* |
| ------------------- | -------------- | -------------- | -------------- | --------- |
| src/pages/index.js  | /**en**        | /**ko**        | /**de**        | /         |
| src/pages/page-2.js | /**en**/page-2 | /**ko**/page-2 | /**de**/page-2 | /page-2   |

**Default Pages and Redirection**

If redirect option is `true`, `/` or `/page-2` will be redirected to the user's preferred language router. e.g) `/ko` or `/ko/page-2`. Otherwise, the pages will render `defaultLangugage` language. You can also specify additional component to be rendered on redirection page by adding `redirectComponent` option.

## Plugin Options

| Option            | Type              | Description                                                                                                                                                                                    |
| ----------------- | ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| path              | string            | language JSON resource path                                                                                                                                                                    |
| languages         | string[]          | supported language keys                                                                                                                                                                        |
| defaultLanguage   | string            | default language when visiting `/page` instead of `ko/page`                                                                                                                                    |
| redirect          | boolean           | if the value is `true`, `/` or `/page-2` will be redirected to the user's preferred language router. e.g) `/ko` or `/ko/page-2`. Otherwise, the pages will render `defaultLangugage` language. |
| redirectComponent | string (optional) | additional component file path to be rendered on with a redirection component for SEO.                                                                                                         |

## Components

To make it easy to handle i18n with multi-language url routes, the plugin provides several components.

To use it, simply import it from `gatsby-plugin-react-intl`.

| Component           | Type      | Description                                                                                                                                                                  |
| ------------------- | --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Link                | component | This is a wrapper around @gatsby’s Link component that adds useful enhancements for multi-language routes. All props are passed through to @gatsby’s Link component.         |
| navigate            | function  | This is a wrapper around @gatsby’s navigate function that adds useful enhancements for multi-language routes. All options are passed through to @gatsby’s navigate function. |
| changeLocale        | function  | A function that replaces your locale. `changeLocale(locale, to = null)`                                                                                                      |
| IntlContextConsumer | component | A context component to get plugin configuration on the component level.                                                                                                      |
| injectIntl          | component | https://github.com/yahoo/react-intl/wiki/API#injection-api                                                                                                                   |
| FormattedMessage    | component | https://github.com/yahoo/react-intl/wiki/Components#string-formatting-components                                                                                             |
| and more...         |           | https://github.com/yahoo/react-intl/wiki/Components                                                                                                                          |

## License

MIT &copy; [Daewoong Moon](https://github.com/wiziple)
