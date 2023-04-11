# Kotlin website
[![Official project][project-badge]][project-url]
[![Qodana Code Quality Check](https://github.com/JetBrains/kotlin-web-site/actions/workflows/qodana-code-quality-check.yml/badge.svg)](https://github.com/JetBrains/kotlin-web-site/actions/workflows/qodana-code-quality-check.yml)

This repository is the source for [https://kotlinlang.org](https://kotlinlang.org).

* [Website structure](#website-structure)
* [Contribution](#contribution)
* [Local deployment](#local-deployment)
* [Feedback and issues](#feedback-and-issues)

<a id="project-structure"></a>

## Website structure 

### Content

|Website page|Source files|
|------------|--------|
| [Main page](https://kotlinlang.org/) | [templates/pages/index.html](templates/pages/index.html) |
| [Kotlin docs](https://kotlinlang.org/docs/home.html) |[docs/topics](docs/topics)| 
| [Community](https://kotlinlang.org/community/) | [pages/community](pages/community) | 
| [Education](https://kotlinlang.org/education/) | [templates/pages/education](templates/pages/education)| 

Note that source files for the [server-side landing page](https://kotlinlang.org/lp/server-side/) and [Kotlin Multiplatform Mobile landing page](https://kotlinlang.org/lp/mobile/) are not publicly available.

#### Sources in different repositories

Source files for the language specification and the docs for coroutines, lincheck, Dokka, and Library creators' guidelines
are stored in separate repositories

| Website page                                                                                     | GitHub repository                                                   |
|--------------------------------------------------------------------------------------------------|---------------------------------------------------------------------|
| [Coroutines docs](https://kotlinlang.org/docs/coroutines-guide.html)                             | [kotlinx.coroutines](https://github.com/Kotlin/kotlinx.coroutines/) |
| [Lincheck docs](https://kotlinlang.org/docs/lincheck-guide.html)                                 | [kotlinx.lincheck](https://github.com/Kotlin/kotlinx-lincheck/)     |
| [Dokka docs](https://kotlinlang.org/docs/dokka-introduction.html)                                | [Dokka](https://github.com/Kotlin/dokka/)                           |
| [Library creators' guidelines](https://kotlinlang.org/docs/jvm-api-guidelines-introduction.html) | [api-guidelines](https://github.com/Kotlin/api-guidelines)          |
| [Language specification](https://kotlinlang.org/spec/introduction.html)                          | [kotlin-spec](https://github.com/Kotlin/kotlin-spec)                |

#### Auto-generated content

[API reference documentation](https://kotlinlang.org/api/latest/jvm/stdlib/) is generated based on comments in the Kotlin code. 
Learn more about [documenting the Kotlin code](https://kotlinlang.org/docs/kotlin-doc.html).

The [Kotlin grammar reference](https://kotlinlang.org/docs/reference/grammar.html) is generated by the [Kotlin grammar generator](https://github.com/Kotlin/website-grammar-generator) from the
[Kotlin grammar definition](https://github.com/Kotlin/kotlin-spec/tree/release/grammar/src/main/antlr).

### Configuration files

|Configuration| File                                                                                 |
|-----|--------------------------------------------------------------------------------------|
|Navigation and structure| [kr.tree](docs/kr.tree) for docs and [_nav.yml](data/_nav.yml) for other pages       |
|Variables, such as release version | [v.list](docs/v.list) for docs and [releases.yml](data/releases.yml) for other pages |
|Community events on the map | [events.xml](data/events.yml)                                                          |
|Video list (outdated) | [videos.yml](data/videos.yml)                                                        |

### Templates

The Kotlin website uses [Jinja2](https://jinja.palletsprojects.com/en/2.11.x/) templates from the [templates](templates) directory.
Note that all Markdown files, except for [docs](docs), are processed as Jinja templates before HTML conversion. 
This allows using all Jinja benefits for Markdown (for example, building URLs with the `url_for` function).

## Contribution

You can contribute to the Kotlin website by sending us a pull request. 
You can also [create a YouTrack issue](https://youtrack.jetbrains.com/newIssue?project=KT) to discuss your suggestion with the Kotlin team.

For the Kotlin documentation, follow [these guidelines on style and formatting](https://docs.google.com/document/d/1mUuxK4xwzs3jtDGoJ5_zwYLaSEl13g_SuhODdFuh2Dc/edit?usp=sharing).

For other pages, follow the complete syntax reference at the [kramdown site](https://kramdown.gettalong.org/syntax.html).
You can also include metadata fields. Learn more about it in the [Jekyll docs](https://jekyllrb.com/docs/front-matter/).

### Kotlin User Group

To add a Kotlin User Group (KUG), proceed the following way:
1. Open the configuration file [user-groups.yml](/data/user-groups.yml).
2. Find a suitable section among existing ones.
3. Add into the selected section a new group with the following keys:
    - `name`, the name of the group.
    - `country`, the name of the country where the group is located. In the case of a virtual group, please use "International" for that. 
    - `url`, the link to the group's web page.
    - `isVirtual`, set this key with `true` value if the group is online only.
    - `position`, the geo-position of the group, defined by pair of keys: `lat` and `lng`. It better to run `scripts/user_group`.
4. If the group is not virtual, you also need to specify a group's position.
   You can do it manually adding `position` key with the `lat` and `lng` values, as next: 

   ```yaml
   position:
     lat: 1.1111111
     lng: 1.1111111
   ```

   or, to run the geo script (`scripts/user_groups_geolocator.py`) that will do it for you.
   You need to obtain GOOGLE_API_KEY and then run the following script:

   ```
   $ GOOGLE_API_KEY="..." python scripts/universities_geolocator.py
   ```

   You can find more details about `GOOGLE_API_KEY` param in [this article by Google](https://developers.google.com/maps/documentation/geocoding/get-api-key).
   The manual way sometimes is better, because it allows you to specify the position more precisely.  

You can see the structure and types of the expected configuration in [the JSON schema](/data/schemas/user-groups.json).
Once you publish a pull request, the changes will be validated by [GitHub Actions Workflow](.github/workflows/validate-user-groups-data.yml) to prevent misconfiguration.

### Community Events

To add an event to the Community Events, do the following: 
1. Fill the event info in the [events.yml](/data/events.yml) with the next:
   - `lang`, two-letter code considering [ISO 639-1 format](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes).
   - `startDate`, in the format 'yyyy-mm-dd'.
   - `endDate`, in the format 'yyyy-mm-dd'. For the on day event fill the same date as in the startDate.
   - `location`, in the form of 'City, Country'. You can omit it for an online event.
   - `online`, set this key with `true` value in case of online event.
   - `speaker`, the speaker's name.
   - `title`, event's title.
   - `subject`, a title of a talk.
   - `url`, link to the event web page.
   You can see the structure and types of the expected configuration in [the JSON schema](/data/schemas/events.json).
2. Publish the changes creating a pull request. The changes will be validated by [GitHub Actions Workflow](.github/workflows/validate-events-data.yml) to prevent misconfiguration.

## Local deployment

Currently, there is no way to deploy the Kotlin website locally. This ticket tracks the effort of adding support for local testing: [KT-47049](https://youtrack.jetbrains.com/issue/KT-47049).

You can contribute to the Kotlin website by sending us a pull request.

## Feedback and issues

You can:

* Report an issue to [our issue tracker](https://youtrack.jetbrains.com/newIssue?project=KT).
* Share feedback in the [#kotlin-website](https://kotlinlang.slack.com/archives/C02B3PECK6E) channel in our Kotlin public Slack ([get an invite](https://surveys.jetbrains.com/s3/kotlin-slack-sign-up)).
* Email us at [doc-feedback@kotlinlang.org](mailto:doc-feedback@kotlinlang.org).

[project-url]: https://confluence.jetbrains.com/display/ALL/JetBrains+on+GitHub
[project-badge]: https://jb.gg/badges/official.svg
[slack-url]: https://slack.kotlinlang.org

## Pages on Next.js

You can find all pages in the [pages](pages) directory.

### Projects structure

- **Components**. The building blocks.
- **Blocks**. Blocks are groups of components joined together to form a relatively complex, distinct section of an interface.
- **Pages**. Each page is associated with a route based on its file name.

### Images in Next.js

Notice that using `next/image` is not possible because Next.js does not support importing images to HTML files (SSG).
Use Img and Svg components from "next-optimized-images" instead.

# Tests

We use Playwright for writing e2e and Screenshot tests.
See https://playwright.dev/ for more details.

## Prerequirements

To run tests locally:
1. Install supported browsers:

   ```
   npx playwright install
   ```

2. Start Dev Server.

## Run Tests

- `yarn test` to run all tests in headless mode locally.
- `yarn test:e2e` to run e2e tests.
- `yarn test:e2e:headed` to run e2e tests in headed mode.
- `yarn test:e2e:debug` to run e2e tests in headed mode with debug.
- `yarn test:e2e:new` to generate the test for the user interactions.
- `yarn ci:e2e` to run e2e test in CI environments.

## Write Tests

To write e2e test, create spec file `/test/e2e/*your-page*.spec.js`.

## API references tests

Some tests focus on protecting the HTML markup of API references from being corrupted by the KTL components in the Dokka template's extension.
To run these tests locally, follow the next steps:
1. Create the `libs` folder in the project.
2. Open the last successful build of each API reference on TeamCity.
3. Download the artifacts of these builds and place them in the `libs` folder by their name, for example, `kotlinx.coroutines`.
4. Up containers `./scripts/dokka/up.sh`.
5. Run test inside container `./scripts/dokka/run.sh` or on the host with one of the scripts below.

To run visual testing, apply one of the next scripts:  
- `yarn test:visual` to compare pages with base screenshots. Base screenshots are in `test/visual`.
- `yarn test:visual:headed` to run visual test in headed mode.
- `yarn test:visual:update` to update all screenshots, for example, when the page has been changed.
- `yarn ci:visual` to run visual test in CI environments. 