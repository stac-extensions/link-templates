# Link Templates Extension Specification

- **Title:** Link Templates
- **Identifier:** <https://stac-extensions.github.io/link-templates/v1.0.0/schema.json>
- **Field Name Prefix:** -
- **Scope:** Item, Collection, Catalog
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @m-mohr

This document explains the Link Templates Extension to the
[SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.

This extension offers a way to properly expose templated links in STAC, which are similar to the `links` array in STAC.
The difference is that the URI can contain variables, so can't be resolved without replacing the variables with specific values.

Potential usecases are exposing links to ZARR chunks or web map links such as XYZ.

All specifications originate from [`linkTemplates` as defined in OGC API - Records](https://docs.ogc.org/DRAFTS/20-004r1.html#sc_templated_links_with_variables).

- Examples:
  - [STAC Item](examples/item.json)
  - [STAC Collection](examples/collection.json)
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)

## Fields

The fields in the table below can be used in these parts of STAC documents:

- [x] Catalogs
- [x] Collections
- [x] Item
- [ ] Item Properties (incl. Summaries in Collections)
- [ ] Assets (for both Collections and Items, incl. Item Asset Definitions in Collections)
- [ ] Links

| Field Name    | Type                                              | Description                                |
| ------------- | ------------------------------------------------- | ------------------------------------------ |
| linkTemplates | \[[Link Templates Object](#link-templates-object)] | **REQUIRED**. An array of templated links. |

In case of doubt, the defintions in OGC API - Records are normative.
[See OGC API - Records for more details.](https://docs.ogc.org/DRAFTS/20-004r1.html#sc_templated_links_with_variables).

### Link Templates Object

The Link Templates Object follows the definition of the STAC
[Link Object](https://github.com/radiantearth/stac-spec/blob/master/commons/links.md#link-object),
except that it has no `href`.

Instead of the `href`, the Link Template Object adds the following fields to the Link Object:

| Field Name  | Type             | Description |
| ----------- | ---------------- | ----------- |
| uriTemplate | string           | **REQUIRED**. Supplies a resolvable URI to a remote resource (or resource fragment). |
| varBase     | string           | The base URI to which the variable name can be appended to retrieve the definition of the variable as a JSON Schema fragment. |
| variables   | Map\<string, \*> | This object contains one key per substitution variable in the templated URL.  Each key defines the schema of one substitution variable using a JSON Schema fragment and can thus include things like the data type of the variable, enumerations, minimum values, maximum values, etc. |

Additionaly, the following fields can be used as defined STAC and OGC API - Records:

- `rel` - **REQUIRED**
- `type`
- `title`
- `created`
- `updated`

The following fields are only defined in the STAC Link Object, but not in OGC API - Records, and can be used as well:

- `method`
- `headers`
- `body`

The following fields are only defined in OGC API - Records, but not in the STAC Link Object, and can be used as well:

- `hreflang`
- `length` (note: [`file:size`](https://github.com/stac-extensions/file) is better suited in a STAC context)
- `profile`

## Relation types

The following types should be used as applicable `rel` types in the
[Link Object](https://github.com/radiantearth/stac-spec/tree/master/item-spec/item-spec.md#link-object).

| Type  | Description                           |
| ----- | ------------------------------------- |
| asset | Identifies link templates that point to asset-like resources. |

The `asset` relation type plugs a gap between OGC API - Records and STAC.
As there's no construct to define templated links in assets and OGC API - Records has no notion of assets,
we use the relation type to enable linked templates for asset-like resources.

## Contributing

All contributions are subject to the
[STAC Specification Code of Conduct](https://github.com/radiantearth/stac-spec/blob/master/CODE_OF_CONDUCT.md).
For contributions, please follow the
[STAC specification contributing guide](https://github.com/radiantearth/stac-spec/blob/master/CONTRIBUTING.md) Instructions
for running tests are copied here for convenience.

### Running tests

The same checks that run as checks on PR's are part of the repository and can be run locally to verify that changes are valid.
To run tests locally, you'll need `npm`, which is a standard part of any [node.js installation](https://nodejs.org/en/download/).

First you'll need to install everything with npm once. Just navigate to the root of this repository and on
your command line run:

```bash
npm install
```

Then to check markdown formatting and test the examples against the JSON schema, you can run:

```bash
npm test
```

This will spit out the same texts that you see online, and you can then go and fix your markdown or examples.

If the tests reveal formatting problems with the examples, you can fix them with:

```bash
npm run format-examples
```
