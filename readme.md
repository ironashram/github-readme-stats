<div align="center">
  <h1 style="font-size: 28px; margin: 10px 0;">GitHub Readme Stats</h1>
  <p>Get dynamically generated GitHub stats on your READMEs!</p>
</div>

<details>
<summary>Table of contents (Click to show)</summary>

- [GitHub Stats Card](#github-stats-card)
    - [Hiding individual stats](#hiding-individual-stats)
    - [Showing additional individual stats](#showing-additional-individual-stats)
    - [Showing icons](#showing-icons)
    - [Showing commits count for specified year](#showing-commits-count-for-specified-year)
    - [Themes](#themes)
    - [Customization](#customization)
- [GitHub Extra Pins](#github-extra-pins)
    - [Usage](#usage)
    - [Options](#options)
- [GitHub Gist Pins](#github-gist-pins)
    - [Usage](#usage-1)
    - [Options](#options-1)
- [Top Languages Card](#top-languages-card)
    - [Usage](#usage-2)
    - [Options](#options-2)
    - [Language stats algorithm](#language-stats-algorithm)
    - [Exclude individual repositories](#exclude-individual-repositories)
    - [Hide individual languages](#hide-individual-languages)
    - [Show more languages](#show-more-languages)
    - [Compact Language Card Layout](#compact-language-card-layout)
    - [Donut Chart Language Card Layout](#donut-chart-language-card-layout)
    - [Donut Vertical Chart Language Card Layout](#donut-vertical-chart-language-card-layout)
    - [Pie Chart Language Card Layout](#pie-chart-language-card-layout)
    - [Hide Progress Bars](#hide-progress-bars)
    - [Change format of language's stats](#change-format-of-languages-stats)
- [WakaTime Stats Card](#wakatime-stats-card)
    - [Options](#options-3)
- [Align the cards](#align-the-cards)
    - [Stats and top languages cards](#stats-and-top-languages-cards)
    - [Pinning repositories](#pinning-repositories)
- [Deploy on your own](#deploy-on-your-own)
  - [GitHub Actions](#github-actions)
  - [Self-hosted (Vercel/Other)](#self-hosted-vercelother)
    - [First step: get your Personal Access Token (PAT)](#first-step-get-your-personal-access-token-pat)
    - [On Vercel](#on-vercel)
    - [On other platforms](#on-other-platforms)
    - [Available environment variables](#available-environment-variables)
  - [Keep your fork up to date](#keep-your-fork-up-to-date)
</details>

All examples below use the endpoint path and query parameters only. Prefix them with your own instance base URL (e.g. `https://your-instance/api?...`).

# GitHub Stats Card

Copy the endpoint into a markdown image on your README, prefixed with your instance URL.

Change the `?username=` value to your GitHub username.

```
/api?username=YOUR_USERNAME
```

> [!NOTE]
> Available ranks are S (top 1%), A+ (12.5%), A (25%), A- (37.5%), B+ (50%), B (62.5%), B- (75%), C+ (87.5%) and C (everyone). This ranking scheme is based on the [Japanese academic grading](https://wikipedia.org/wiki/Academic_grading_in_Japan) system. The global percentile is calculated as a weighted sum of percentiles for each statistic (number of commits, pull requests, reviews, issues, stars, and followers), based on the cumulative distribution function of the [exponential](https://wikipedia.org/wiki/exponential_distribution) and the [log-normal](https://wikipedia.org/wiki/Log-normal_distribution) distributions. The implementation can be investigated at [src/calculateRank.js](src/calculateRank.js). The circle around the rank shows 100 minus the global percentile.

### Hiding individual stats

You can pass a query parameter `&hide=` to hide any specific stats with comma-separated values.

> Options: `&hide=stars,commits,prs,issues,contribs`

```
/api?username=YOUR_USERNAME&hide=contribs,prs
```

### Showing additional individual stats

You can pass a query parameter `&show=` to show any specific additional stats with comma-separated values.

> Options: `&show=reviews,discussions_started,discussions_answered,prs_merged,prs_merged_percentage`

```
/api?username=YOUR_USERNAME&show=reviews,discussions_started,discussions_answered,prs_merged,prs_merged_percentage
```

### Showing icons

To enable icons, you can pass `&show_icons=true` in the query param, like so:

```
/api?username=YOUR_USERNAME&show_icons=true
```

### Showing commits count for specified year

You can specify a year and fetch only the commits that were made in that year by passing `&commits_year=YYYY` to the parameter.

```
/api?username=YOUR_USERNAME&commits_year=2020
```

### Themes

With inbuilt themes, you can customize the look of the card without doing any [manual customization](#customization).

Use `&theme=THEME_NAME` parameter like so :

```
/api?username=YOUR_USERNAME&show_icons=true&theme=radical
```

#### All inbuilt themes

GitHub Readme Stats comes with several built-in themes (e.g. `dark`, `radical`, `merko`, `gruvbox`, `tokyonight`, `onedark`, `cobalt`, `synthwave`, `highcontrast`, `dracula`).

You can look at a preview for [all available themes](themes/README.md) or checkout the [theme config file](themes/index.js). Please note that the addition of new themes is paused to decrease maintenance efforts; all pull requests related to new themes will be closed.

#### Responsive Card Theme

Since GitHub re-uploads the cards and serves them from their [CDN](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/about-anonymized-urls), the browser/GitHub theme can not be inferred on the server side. There are, however, four methods you can use to create dynamic themes on the client side.

##### Use the transparent theme

A `transparent` theme with a transparent background is included. It is optimized to look good on GitHub's dark and light default themes. Enable it using the `&theme=transparent` parameter:

```
/api?username=YOUR_USERNAME&show_icons=true&theme=transparent
```

##### Add transparent alpha channel to a theme's bg_color

You can use the `bg_color` parameter to make any of [the available themes](themes/README.md) transparent. This is done by setting the `bg_color` to a color with a transparent alpha channel (i.e. `bg_color=00000000`):

```
/api?username=YOUR_USERNAME&show_icons=true&bg_color=00000000
```

##### Use GitHub's theme context tag

You can use [GitHub's theme context](https://github.blog/changelog/2021-11-24-specify-theme-context-for-images-in-markdown/) tags to switch the theme based on the user's GitHub theme automatically. This is done by appending `#gh-dark-mode-only` or `#gh-light-mode-only` to the end of an image URL, defining whether the image is only shown to viewers using a light or a dark GitHub theme:

```
/api?username=YOUR_USERNAME&show_icons=true&theme=dark#gh-dark-mode-only
/api?username=YOUR_USERNAME&show_icons=true&theme=default#gh-light-mode-only
```

##### Use GitHub's media feature

You can use [GitHub's media feature](https://github.blog/changelog/2022-05-19-specify-theme-context-for-images-in-markdown-beta/) in HTML to specify whether to display images for light or dark themes. This is done using the HTML `<picture>` element combined with the `prefers-color-scheme` media feature (prefix each `srcset` with your instance URL):

```html
<picture>
  <source
    srcset="/api?username=YOUR_USERNAME&show_icons=true&theme=dark"
    media="(prefers-color-scheme: dark)"
  />
  <source
    srcset="/api?username=YOUR_USERNAME&show_icons=true"
    media="(prefers-color-scheme: light), (prefers-color-scheme: no-preference)"
  />
  <img src="/api?username=YOUR_USERNAME&show_icons=true" />
</picture>
```

### Customization

You can customize the appearance of all your cards however you wish with URL parameters.

#### Common Options

| Name | Description | Type | Default value |
| --- | --- | --- | --- |
| `title_color` | Card's title color. | string (hex color) | `2f80ed` |
| `text_color` | Body text color. | string (hex color) | `434d58` |
| `icon_color` | Icons color if available. | string (hex color) | `4c71f2` |
| `border_color` | Card's border color. Does not apply when `hide_border` is enabled. | string (hex color) | `e4e2e2` |
| `bg_color` | Card's background color. | string (hex color or a gradient in the form of *angle,start,end*) | `fffefe` |
| `hide_border` | Hides the card's border. | boolean | `false` |
| `theme` | Name of the theme, choose from [all available themes](themes/README.md). | enum | `default` |
| `cache_seconds` | Sets the cache header manually (min: 21600, max: 86400). | integer | `21600` |
| `locale` | Sets the language in the card, you can check full list of available locales [here](#available-locales). | enum | `en` |
| `border_radius` | Corner rounding on the card. | number | `4.5` |

> [!NOTE]
> Default cache durations are: stats card 24 hours, top languages card 144 hours (6 days), pin card 240 hours (10 days), gist card 48 hours (2 days), and wakatime card 24 hours. Set the [environment variable](#available-environment-variables) `CACHE_SECONDS` on your instance to override them.

##### Gradient in bg_color

You can provide multiple comma-separated values in the bg_color option to render a gradient with the following format:

    &bg_color=DEG,COLOR1,COLOR2,COLOR3...COLOR10

##### Available locales

Here is a list of all available locales:

<table>
<tr><td>

| Code | Locale |
| --- | --- |
| `ar` | Arabic |
| `az` | Azerbaijani |
| `bn` | Bengali |
| `bg` | Bulgarian |
| `my` | Burmese |
| `ca` | Catalan |
| `cn` | Chinese |
| `zh-tw` | Chinese (Taiwan) |
| `cs` | Czech |
| `nl` | Dutch |
| `en` | English |
| `fil` | Filipino |
| `fi` | Finnish |
| `fr` | French |
| `de` | German |
| `el` | Greek |

</td><td>

| Code | Locale |
| --- | --- |
| `he` | Hebrew |
| `hi` | Hindi |
| `hu` | Hungarian |
| `id` | Indonesian |
| `it` | Italian |
| `ja` | Japanese |
| `kr` | Korean |
| `ml` | Malayalam |
| `np` | Nepali |
| `no` | Norwegian |
| `fa` | Persian (Farsi) |
| `pl` | Polish |
| `pt-br` | Portuguese (Brazil) |
| `pt-pt` | Portuguese (Portugal) |
| `ro` | Romanian |

</td><td>

| Code | Locale |
| --- | --- |
| `ru` | Russian |
| `sa` | Sanskrit |
| `sr` | Serbian (Cyrillic) |
| `sr-latn` | Serbian (Latin) |
| `sk` | Slovak |
| `es` | Spanish |
| `sw` | Swahili |
| `se` | Swedish |
| `ta` | Tamil |
| `th` | Thai |
| `tr` | Turkish |
| `uk-ua` | Ukrainian |
| `ur` | Urdu |
| `uz` | Uzbek |
| `vi` | Vietnamese |

</td></tr>
</table>

If your language is not supported, please consider contributing! You can find more information about how to do it in the [contributing guidelines](CONTRIBUTING.md#translations-contribution).

#### Stats Card Exclusive Options

| Name | Description | Type | Default value |
| --- | --- | --- | --- |
| `hide` | Hides the [specified items](#hiding-individual-stats) from stats. | string (comma-separated values) | `null` |
| `hide_title` | Hides the title of your stats card. | boolean | `false` |
| `card_width` | Sets the card's width manually. | number | `500px  (approx.)` |
| `hide_rank` | Hides the rank and automatically resizes the card width. | boolean | `false` |
| `rank_icon` | Shows alternative rank icon (i.e. `github`, `percentile` or `default`). | enum | `default` |
| `show_icons` | Shows icons near all stats. | boolean | `false` |
| `include_all_commits` | Count total commits instead of just the current year commits. | boolean | `false` |
| `line_height` | Sets the line height between text. | integer | `25` |
| `exclude_repo` | Excludes specified repositories. | string (comma-separated values) | `null` |
| `custom_title` | Sets a custom title for the card. | string | `<username> GitHub Stats` |
| `text_bold` | Uses bold text. | boolean | `true` |
| `disable_animations` | Disables all animations in the card. | boolean | `false` |
| `ring_color` | Color of the rank circle. | string (hex color) | `2f80ed` |
| `number_format` | Switches between two available formats for displaying the card values `short` (i.e. `6.6k`) and `long` (i.e. `6626`). | enum | `short` |
| `number_precision` | Enforce the number of digits after the decimal point for `short` number format. Must be an integer between 0 and 2. Will be ignored for `long` number format. | integer (0, 1 or 2) | `null` |
| `show` | Shows [additional items](#showing-additional-individual-stats) on stats card (i.e. `reviews`, `discussions_started`, `discussions_answered`, `prs_merged` or `prs_merged_percentage`). | string (comma-separated values) | `null` |
| `commits_year` | Filters and counts only commits made in the specified year. | integer _(YYYY)_ | `<current year> (one year to date)` |

> [!WARNING]
> Custom title should be URI-escaped, as specified in [Percent Encoding](https://en.wikipedia.org/wiki/Percent-encoding) (i.e: `Anurag's GitHub Stats` should become `Anurag%27s%20GitHub%20Stats`). You can use [urlencoder.org](https://www.urlencoder.org/) to help you do this automatically.

> [!NOTE]
> When hide_rank=`true`, the minimum card width is 270 px + the title length and padding.

***

# GitHub Extra Pins

GitHub extra pins allow you to pin more than 6 repositories in your profile using a GitHub readme profile.

Yay! You are no longer limited to 6 pinned repositories.

### Usage

Copy the endpoint into your readme and change the parameters.

```
/api/pin/?username=YOUR_USERNAME&repo=REPO_NAME
```

### Options

You can customize the appearance and behavior of the pinned repository card using the [common options](#common-options) and exclusive options listed in the table below.

| Name | Description | Type | Default value |
| --- | --- | --- | --- |
| `show_owner` | Shows the repo's owner name. | boolean | `false` |
| `description_lines_count` | Manually set the number of lines for the description. Specified value will be clamped between 1 and 3. If this parameter is not specified, the number of lines will be automatically adjusted according to the actual length of the description. | number | `null` |

Use the `show_owner` query option to include the repo's owner username.

```
/api/pin/?username=YOUR_USERNAME&repo=REPO_NAME&show_owner=true
```

# GitHub Gist Pins

GitHub gist pins allow you to pin gists in your GitHub profile using a GitHub readme profile.

### Usage

Copy the endpoint into your readme and change the gist id.

```
/api/gist?id=GIST_ID
```

### Options

You can customize the appearance and behavior of the gist card using the [common options](#common-options) and exclusive options listed in the table below.

| Name | Description | Type | Default value |
| --- | --- | --- | --- |
| `show_owner` | Shows the gist's owner name. | boolean | `false` |

Use the `show_owner` query option to include the gist's owner username.

```
/api/gist?id=GIST_ID&show_owner=true
```

# Top Languages Card

The top languages card shows a GitHub user's most frequently used languages.

> [!NOTE]
> Top Languages does not indicate the user's skill level or anything like that; it's a GitHub metric to determine which languages have the most code on GitHub.

> [!WARNING]
> This card shows language usage only inside your own non-forked repositories, not depending on who the author of the commits is. It does not include your contributions to other users/organizations repositories, as there is currently no way to get this data from the GitHub API.

> [!WARNING]
> This card shows data only about the first 100 repositories, due to GitHub API limitations.

### Usage

Copy the endpoint into your readme and change the parameters.

```
/api/top-langs/?username=YOUR_USERNAME
```

### Options

You can customize the appearance and behavior of the top languages card using the [common options](#common-options) and exclusive options listed in the table below.

| Name | Description | Type | Default value |
| --- | --- | --- | --- |
| `hide` | Hides the [specified languages](#hide-individual-languages) from card. | string (comma-separated values) | `null` |
| `hide_title` | Hides the title of your card. | boolean | `false` |
| `layout` | Switches between five available layouts `normal` & `compact` & `donut` & `donut-vertical` & `pie`. | enum | `normal` |
| `card_width` | Sets the card's width manually. | number | `300` |
| `langs_count` | Shows more languages on the card, between 1-20. | integer | `5` for `normal` and `donut`, `6` for other layouts |
| `exclude_repo` | Excludes specified repositories. | string (comma-separated values) | `null` |
| `custom_title` | Sets a custom title for the card. | string | `Most Used Languages` |
| `disable_animations` | Disables all animations in the card. | boolean | `false` |
| `hide_progress` | Uses the compact layout option, hides percentages, and removes the bars. | boolean | `false` |
| `size_weight` | Configures language stats algorithm (see [Language stats algorithm](#language-stats-algorithm)). | integer | `1` |
| `count_weight` | Configures language stats algorithm (see [Language stats algorithm](#language-stats-algorithm)). | integer | `0` |
| `stats_format` | Switches between two available formats for language's stats `percentages` and `bytes`. | enum | `percentages` |

> [!WARNING]
> Language names and custom title should be URI-escaped, as specified in [Percent Encoding](https://en.wikipedia.org/wiki/Percent-encoding) (i.e: `c++` should become `c%2B%2B`, `jupyter notebook` should become `jupyter%20notebook`, `Most Used Languages` should become `Most%20Used%20Languages`, etc.) You can use [urlencoder.org](https://www.urlencoder.org/) to help you do this automatically.

### Language stats algorithm

The following algorithm is used to calculate the language percentages on the language card:

```js
ranking_index = (byte_count ^ size_weight) * (repo_count ^ count_weight)
```

By default, only the byte count is used for determining the language percentages shown on the language card (i.e. `size_weight=1` and `count_weight=0`). You can, however, use the `&size_weight=` and `&count_weight=` options to weight the language usage calculation. The values must be positive real numbers.

*   `&size_weight=1&count_weight=0` - *(default)* Orders by byte count.
*   `&size_weight=0.5&count_weight=0.5` - *(recommended)* Uses both byte and repo count for ranking.
*   `&size_weight=0&count_weight=1` - Orders by repo count.

```
/api/top-langs/?username=YOUR_USERNAME&size_weight=0.5&count_weight=0.5
```

### Exclude individual repositories

You can use the `&exclude_repo=repo1,repo2` parameter to exclude individual repositories.

```
/api/top-langs/?username=YOUR_USERNAME&exclude_repo=repo1,repo2
```

### Hide individual languages

You can use `&hide=language1,language2` parameter to hide individual languages.

```
/api/top-langs/?username=YOUR_USERNAME&hide=javascript,html
```

### Show more languages

You can use the `&langs_count=` option to increase or decrease the number of languages shown on the card. Valid values are integers between 1 and 20 (inclusive). By default it is `5` for `normal` & `donut` and `6` for other layouts.

```
/api/top-langs/?username=YOUR_USERNAME&langs_count=8
```

### Compact Language Card Layout

You can use the `&layout=compact` option to change the card design.

```
/api/top-langs/?username=YOUR_USERNAME&layout=compact
```

### Donut Chart Language Card Layout

You can use the `&layout=donut` option to change the card design.

```
/api/top-langs/?username=YOUR_USERNAME&layout=donut
```

### Donut Vertical Chart Language Card Layout

You can use the `&layout=donut-vertical` option to change the card design.

```
/api/top-langs/?username=YOUR_USERNAME&layout=donut-vertical
```

### Pie Chart Language Card Layout

You can use the `&layout=pie` option to change the card design.

```
/api/top-langs/?username=YOUR_USERNAME&layout=pie
```

### Hide Progress Bars

You can use the `&hide_progress=true` option to hide the percentages and the progress bars (layout will be automatically set to `compact`).

```
/api/top-langs/?username=YOUR_USERNAME&hide_progress=true
```

### Change format of language's stats

You can use the `&stats_format=bytes` option to display the stats in bytes instead of percentage.

```
/api/top-langs/?username=YOUR_USERNAME&stats_format=bytes
```

# WakaTime Stats Card

> [!WARNING]
> Only data from public WakaTime profiles is shown. You therefore have to make sure that **BOTH** `Display code time publicly` and `Display languages, editors, os, categories publicly` are enabled.

> [!WARNING]
> If you just created a new WakaTime account, it might take up to 24 hours until your stats become visible on the WakaTime stats card.

Change the `?username=` value to your [WakaTime](https://wakatime.com) username.

```
/api/wakatime?username=YOUR_WAKATIME_USERNAME
```

### Options

You can customize the appearance and behavior of the WakaTime stats card using the [common options](#common-options) and exclusive options listed in the table below.

| Name | Description | Type | Default value |
| --- | --- | --- | --- |
| `hide` | Hides the languages specified from the card. | string (comma-separated values) | `null` |
| `hide_title` | Hides the title of your card. | boolean | `false` |
| `card_width` | Sets the card's width manually. | number | `495` |
| `line_height` | Sets the line height between text. | integer | `25` |
| `hide_progress` | Hides the progress bar and percentage. | boolean | `false` |
| `custom_title` | Sets a custom title for the card. | string | `WakaTime Stats` |
| `layout` | Switches between two available layouts `default` & `compact`. | enum | `default` |
| `langs_count` | Limits the number of languages on the card, defaults to all reported languages. | integer | `null` |
| `api_domain` | Sets a custom API domain for the card, e.g. to use services like [Hakatime](https://github.com/mujx/hakatime) or [Wakapi](https://github.com/muety/wakapi) | string | `wakatime.com` |
| `display_format` | Sets the WakaTime stats display format. Choose `time` to display time-based stats or `percent` to show percentages. | enum | `time` |
| `disable_animations` | Disables all animations in the card. | boolean | `false` |

> [!WARNING]
> Custom title should be URI-escaped, as specified in [Percent Encoding](https://en.wikipedia.org/wiki/Percent-encoding) (i.e: `WakaTime Stats` should become `WakaTime%20Stats`). You can use [urlencoder.org](https://www.urlencoder.org/) to help you do this automatically.

***

# Align the cards

By default, GitHub does not lay out the cards side by side. To do that, you can use the following approaches (prefix each `src` with your instance URL).

### Stats and top languages cards

```html
<a href="https://github.com/YOUR_USERNAME">
  <img height=200 align="center" src="/api?username=YOUR_USERNAME" />
</a>
<a href="https://github.com/YOUR_USERNAME">
  <img height=200 align="center" src="/api/top-langs?username=YOUR_USERNAME&layout=compact&langs_count=8&card_width=320" />
</a>
```

### Pinning repositories

```html
<a href="https://github.com/YOUR_USERNAME">
  <img align="center" src="/api/pin/?username=YOUR_USERNAME&repo=REPO_ONE" />
</a>
<a href="https://github.com/YOUR_USERNAME">
  <img align="center" src="/api/pin/?username=YOUR_USERNAME&repo=REPO_TWO" />
</a>
```

# Deploy on your own

Self-deploy via GitHub Actions or your own hosted instance. GitHub Actions is the simplest setup with static SVGs stored in your repo but less frequent updates, while self-hosting takes more work and can serve fresher stats (with caching).

## GitHub Actions

GitHub Actions generates static SVGs and avoids per-request API calls. By default it uses `GITHUB_TOKEN` (public stats only); for private stats, set a [PAT](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens) as a secret and pass it to the action instead.

Create `/.github/workflows/grs.yml` in your profile repo (`USERNAME/USERNAME`):

```yaml
name: Update README cards

on:
  schedule:
    - cron: "0 3 * * *"
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Generate stats card
        uses: readme-tools/github-readme-stats-action@v1
        with:
          card: stats
          options: username=${{ github.repository_owner }}&show_icons=true
          path: profile/stats.svg
          token: ${{ secrets.GITHUB_TOKEN }}

      - name: Commit cards
        run: |
          git config user.name "github-actions"
          git config user.email "github-actions@users.noreply.github.com"
          git add profile/*.svg
          git commit -m "Update README cards" || exit 0
          git push
```

Then embed from your profile README:

```md
![Stats](./profile/stats.svg)
```

See more options and examples in the [GitHub Readme Stats Action README](https://github.com/readme-tools/github-readme-stats-action#readme).

## Self-hosted (Vercel/Other)

Running your own instance gives you full control over caching, tokens, and private stats.

### First step: get your Personal Access Token (PAT)

For deploying your own instance of GitHub Readme Stats, you will need to create a GitHub Personal Access Token (PAT). Below are the steps to create one and the scopes you need to select for both classic and fine-grained tokens.

Selecting the right scopes for your token is important in case you want to display private contributions on your cards.

#### Classic token

* Go to [Account -> Settings -> Developer Settings -> Personal access tokens -> Tokens (classic)](https://github.com/settings/tokens).
* Click on `Generate new token -> Generate new token (classic)`.
* Scopes to select:
  * repo
  * read:user
  * read:org (required for counting stars of org-owned repositories, see <code>INCLUDE_ORG_STARS</code>)
* Click on `Generate token` and copy it.

#### Fine-grained token

> [!WARNING]
> This limits the scope to issues in your repositories and includes only public commits.

* Go to [Account -> Settings -> Developer Settings -> Personal access tokens -> Fine-grained tokens](https://github.com/settings/tokens).
* Click on `Generate new token -> Generate new token`.
* Select an expiration date.
* Select `All repositories`.
* Scopes to select in `Repository permission`:
  * Commit statuses: read-only
  * Contents: read-only
  * Issues: read-only
  * Metadata: read-only
  * Pull requests: read-only
* Click on `Generate token` and copy it.

### On Vercel

Deploy your own Vercel instance so requests use your own token and rate limits.

<details>
 <summary><b>Step-by-step guide on setting up your own Vercel instance</b></summary>

1.  Go to [vercel.com](https://vercel.com/).
2.  Click on `Log in`.
3.  Sign in with GitHub by pressing `Continue with GitHub`.
4.  Sign in to GitHub and allow access to all repositories if prompted.
5.  Fork this repo.
6.  Go back to your [Vercel dashboard](https://vercel.com/dashboard).
7.  To import a project, click the `Add New...` button and select the `Project` option.
8.  Click the `Continue with GitHub` button, search for the required Git Repository and import it by clicking the `Import` button. Alternatively, you can import a Third-Party Git Repository using the `Import Third-Party Git Repository ->` link at the bottom of the page.
9.  Create a Personal Access Token (PAT) as described in the [previous section](#first-step-get-your-personal-access-token-pat).
10. Add the PAT as an environment variable named `PAT_1` (as shown).
11. Click deploy, and you're good to go. See your domains to use the API!

</details>

> [!NOTE]
> If you are on the [Pro (i.e. paid)](https://vercel.com/pricing) Vercel plan, the [maxDuration](https://vercel.com/docs/concepts/projects/project-configuration#value-definition) value in [vercel.json](vercel.json) can be increased if your Vercel instance frequently times out during the card request. Keep this value lower than `30` seconds to prevent high memory usage.

### On other platforms

> [!WARNING]
> This way of using GRS is not officially supported and was added to cater to some particular use cases where Vercel could not be used. Support for this method is therefore limited.

<details>
<summary><b>Step-by-step guide for deploying on other platforms</b></summary>

1.  Fork or clone this repo as per your needs.
2.  Move `express` from the devDependencies to the dependencies section of `package.json`.
3.  Run `npm i` if needed (initial setup).
4.  Run `node express.js` to start the server, or set the entry point to `express.js` in `package.json` if you're deploying on a managed service.
5.  You're done.

</details>

### Available environment variables

GitHub Readme Stats provides several environment variables that can be used to customize the behavior of your self-hosted instance. These include:

<table>
  <thead>
    <tr>
      <th>Name</th>
      <th>Description</th>
      <th>Supported values</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>CACHE_SECONDS</code></td>
      <td>Sets the cache duration in seconds for the generated cards. This variable takes precedence over the default cache timings. If this variable is not set, the default cache duration is 24 hours (86,400 seconds).</td>
      <td>Any positive integer or <code>0</code> to disable caching</td>
    </tr>
    <tr>
      <td><code>WHITELIST</code></td>
      <td>A comma-separated list of GitHub usernames that are allowed to access your instance. If this variable is not set, all usernames are allowed.</td>
      <td>Comma-separated GitHub usernames</td>
    </tr>
    <tr>
      <td><code>GIST_WHITELIST</code></td>
      <td>A comma-separated list of GitHub Gist IDs that are allowed to be accessed on your instance. If this variable is not set, all Gist IDs are allowed.</td>
      <td>Comma-separated GitHub Gist IDs</td>
    </tr>
    <tr>
      <td><code>EXCLUDE_REPO</code></td>
      <td>A comma-separated list of repositories that will be excluded from stats and top languages cards on your instance. This allows repository exclusion without exposing repository names in public URLs. This enhances privacy for self-hosted instances that include private repositories in stats cards.</td>
      <td>Comma-separated repository names</td>
    </tr>
    <tr>
      <td><code>FETCH_MULTI_PAGE_STARS</code></td>
      <td>Enables fetching all starred repositories for accurate star counts, especially for users with more than 100 repositories. This may increase response times and API points usage.</td>
      <td><code>true</code> or <code>false</code></td>
    </tr>
    <tr>
      <td><code>INCLUDE_ORG_STARS</code></td>
      <td>Counts stars of repositories owned by organizations the user can administer on the stats card. Enabled by default; set to <code>false</code> to count only user-owned repositories.</td>
      <td><code>true</code> or <code>false</code></td>
    </tr>
  </tbody>
</table>

See [the Vercel documentation](https://vercel.com/docs/concepts/projects/environment-variables) on adding these environment variables to your Vercel instance.

> [!WARNING]
> Remember to redeploy your instance after making any changes to the environment variables so that the updates take effect. The changes will not be applied to the previous deployments.

## Keep your fork up to date

You can keep your fork, and thus your private instance, up to date with the upstream using GitHub's [Sync Fork button](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/syncing-a-fork). You can also use the [pull](https://github.com/wei/pull) package created by [@wei](https://github.com/wei) to automate this process.
