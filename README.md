<div align="center">

[![logo](src/img/logo-b.webp)](https://github.com/pkgforge-dev/backage)

# [backage](https://github.com/pkgforge-dev/backage)

**It's all part and parcel**

---

[![packages](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fgithub.com%2Fpkgforge-dev%2Fbackage%2Fraw%2Findex%2F.json&query=%24.packages&logo=github&logoColor=959da5&label=packages&labelColor=333a41&color=grey)](https://github.com/pkgforge-dev/backage/tree/index) [![build](https://github.com/pkgforge-dev/backage/actions/workflows/publish.yml/badge.svg)](https://github.com/pkgforge-dev/backage/pkgs/container/backage) [![built](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fgithub.com%2Fpkgforge-dev%2Fbackage%2Fraw%2Findex%2F.json&query=%24.date&logo=github&logoColor=959da5&label=built&labelColor=333a41&color=purple)](https://github.com/pkgforge-dev/backage/releases/latest)

</div>

Wish you could show npm, gem, mvn, Gradle, NuGet, or GHCR badges? Or just query for the download counts? This endpoint makes that possible, using only free GitHub resources; the GitHub Packages API doesn't, and has never, exposed the publicly-available metadata that other registries provide.

## The Endpoint

```py
https://ipitio.github.io/backage/OWNER/[REPO/[PACKAGE]].FORMAT
```

If this is [`ipitio/backage`](https://github.com/ipitio/backage), all you have to do is **star the repo to have GitHub generate this additional endpoint for your public packages!** A service ran by GitHub will add them to its circular priority queue within the next few hours and update the [closed-loop system](https://github.com/pkgforge-dev/backage/releases/latest). Additionally watching and forking the repo, and following the owner, are ways to increase your priority. Yes, I know, but these are the signals that can be automatically processed. Otherwise, if this is a fork or you'd prefer an alternative method, see [below](#manual-actions).

Once added, replace the parameters above with their respective values, scoping to your parsing needs, then access the latest data however you want. Use something like [shields.io/json](https://shields.io/badges/dynamic-json-badge) or [shields.io/xml](https://shields.io/badges/dynamic-xml-badge) to make badges like the ones you just scrolled past or the one [here](https://github.com/badges/shields/issues/5594#issuecomment-2157626147).

> [!NOTE]
> The format can be either `json` or `xml`. You'll need the XML endpoint to evaluate expressions, like filters, with Shields -- see [this issue](https://github.com/ipitio/backage/issues/23).

> [!TIP]
> Use the proxy to convert external JSON to XML! This doesn't currently work with Shields, though.

### The Data

You'll find these properties for packages and their versions:

<details>

<summary>Package</summary>

|       Property        |     Type     | Description                                             |
| :-------------------: | :----------: | ------------------------------------------------------- |
|      `owner_id`       |    number    | The ID of the owner                                     |
|     `owner_type`      |    string    | The type of owner (e.g. `users`)                        |
|    `package_type`     |    string    | The type of package (e.g. `container`)                  |
|        `owner`        |    string    | The owner of the package                                |
|        `repo`         |    string    | The repository of the package                           |
|       `package`       |    string    | The package name                                        |
|        `date`         |    string    | The most recent date the package was refreshed          |
|        `size`         |    string    | Formatted size of the latest version                    |
|      `versions`       |    string    | Formatted count of all versions recently tracked        |
|       `tagged`        |    string    | Formatted count of all tagged versions recently tracked |
|     `owner_rank`      |    string    | Formatted rank by downloads within the owner            |
|      `repo_rank`      |    string    | Formatted rank by downloads within the repository       |
|      `downloads`      |    string    | Formatted count of all downloads                        |
|   `downloads_month`   |    string    | Formatted count of all downloads in the last month      |
|   `downloads_week`    |    string    | Formatted count of all downloads in the last week       |
|    `downloads_day`    |    string    | Formatted count of all downloads in the last day        |
|      `raw_size`       |    number    | Size of the latest version, in bytes                    |
|    `raw_versions`     |    number    | Count of versions ever tracked                          |
|     `raw_tagged`      |    number    | Count of tagged versions ever tracked                   |
|   `raw_owner_rank`    |    number    | Rank by downloads within the owner                      |
|    `raw_repo_rank`    |    number    | Rank by downloads within the repository                 |
|    `raw_downloads`    |    number    | Count of all downloads                                  |
| `raw_downloads_month` |    number    | Count of all downloads in the last month                |
| `raw_downloads_week`  |    number    | Count of all downloads in the last week                 |
|  `raw_downloads_day`  |    number    | Count of all downloads in the last day                  |
|       `version`       | object array | The versions of the package (see below)                 |

</details>

<details>

<summary>Version</summary>

|       Property        |     Type     | Description                                    |
| :-------------------: | :----------: | ---------------------------------------------- |
|         `id`          |    number    | The ID of the version                          |
|        `name`         |    string    | The version name                               |
|        `date`         |    string    | The most recent date the version was refreshed |
|       `newest`        |   boolean    | Whether the version is the newest              |
|       `latest`        |   boolean    | Whether the version is the newest tagged       |
|        `size`         |    string    | Formatted size of the version                  |
|      `downloads`      |    string    | Formatted count of downloads                   |
|   `downloads_month`   |    string    | Formatted count of downloads in the last month |
|   `downloads_week`    |    string    | Formatted count of downloads in the last week  |
|    `downloads_day`    |    string    | Formatted number of downloads in the last day  |
|      `raw_size`       |    number    | Size of the version, in bytes                  |
|    `raw_downloads`    |    number    | Count of downloads                             |
| `raw_downloads_month` |    number    | Count of downloads in the last month           |
| `raw_downloads_week`  |    number    | Count of downloads in the last week            |
|  `raw_downloads_day`  |    number    | Count of downloads in the last day             |
|        `tags`         | string array | The tags of the version                        |

</details>

They can be queried with the following paths:

<details>

<summary>JSON</summary>

You can query a package for its properties, like size or version:

```js
$.PROPERTY
```

```js
$.size
```

Versions may be filtered in and tags out:

```js
$.version[FILTER].PROPERTY
```

```js
$.version[?(@.latest)].tags[?(@!="latest")]
```

As can packages in `owner[/repo]/.json` files:

```js
$.[FILTER].PROPERTY
```

</details>

<details>

<summary>XML</summary>

You can query a package for its properties, like size or version:

```py
/xml/PROPERTY
```

```py
/xml/size
```

Versions can be filtered in and tags out:

```py
/xml/version[FILTER]/PROPERTY
```

```py
/xml/version[./latest[.="true"]]/tags[.!="latest"]
```

As can packages in `owner[/repo]/.xml` files:

```py
/xml/package[FILTER]/PROPERTY
```

</details>

### Manual Actions

To add any other users or organizations not yet [in the index](https://github.com/pkgforge-dev/backage/tree/index), add the case-sensitive name of each one on a new line at the top of `owners.txt` on your own fork [here](https://github.com/pkgforge-dev/backage/edit/master/owners.txt) and make a pull request. Please add just the name(s) -- ids, repos, and packages will be found automatically! If you'd like the service to forget and ignore any subset of packages, add `owner[/repo[/package]]` to `optout.txt` [here](https://github.com/pkgforge-dev/backage/edit/master/optout.txt) and make a pull request.

You can create an independent instance for them that'll update faster and more frequently. Simply fork just the `master` branch, then enable Actions from its tab and all disabled workflows. Your own packages will be picked up automatically! If you previously had to edit the `txt` files, you can do so again after the first run. I recommend that you first confirm here that the packages you're interested in are picked up without issue. This centralized repo will then serve as a backup.

### Alternative URL

```py
https://github.com/pkgforge-dev/backage/raw/index/OWNER/[REPO/[PACKAGE]].FORMAT
```

The endpoint is also available here!

## JSON2XML Proxy

```py
https://ipitio.github.io/backage?json=https://URL/ENCODED/JSON
```

Use your own JSON endpoint with this proxy to convert it into XML. Try it out in your browser:

**<https://ipitio.github.io/backage?json=https://raw.githubusercontent.com/pkgforge-dev/backage/index/.json>**
