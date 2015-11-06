# JSON-LD Frames for DCAT catalogs and datasets

The two JSON-LD frames in this repository are meant for easily working with arbitrary DCAT catalog or dataset resources published as JSON-LD (or converted to it).

The typical workflow is to apply one of the two frames to a given JSON-LD resource, followed by compaction with the context of the resulting framed document. Those are standard operations supported by most JSON-LD processors.

## Features

- short aliases for all common fields
- proper hierarchical structure
- URLs are never automatically turned into relative ones
- optionally translated titles and descriptions via language maps ("_i18n" fields)
- dataset relationships via "isPartOf" and "parts" are always just URI references (never embedded)
- "datasets" and "distributions" are always an array (empty if none exist)
- "keywords" and "parts" are always an array if existing (omitted otherwise)

## Example

### Input

```js
{
  "@graph": [
    {
      "@id": "_:b0",
      "@type": "http://www.w3.org/ns/dcat#Catalog",
      "http://purl.org/dc/terms/title": {
        "@language": "en",
        "@value": "Test Datasets"
      },
      "http://www.w3.org/ns/dcat#dataset": {
        "@id": "http://melodiesproject.eu/datasets/wp2/test"
      }
    },
    {
      "@id": "_:b1",
      "@type": "http://purl.org/dc/terms/Location",
      "http://www.w3.org/ns/locn#geometry": {
        "@type": "http://www.opengis.net/ont/geosparql#wktLiteral",
        "@value": "POLYGON((-180 -90, 180 -90, 180 90, -180 90, -180 -90))"
      }
    },
    {
      "@id": "_:b2",
      "@type": "http://purl.org/dc/terms/PeriodOfTime",
      "http://schema.org/endDate": {
        "@type": "http://www.w3.org/2001/XMLSchema#dateTime",
        "@value": "2015-12-31"
      },
      "http://schema.org/startDate": {
        "@type": "http://www.w3.org/2001/XMLSchema#dateTime",
        "@value": "2015-01-01"
      }
    },
    {
      "@id": "_:b3",
      "@type": "http://www.w3.org/ns/dcat#Distribution",
      "http://purl.org/dc/terms/format": "geojson",
      "http://purl.org/dc/terms/title": {
        "@language": "en",
        "@value": "UK Admin 2 Boundaries as GeoJSON"
      },
      "http://www.w3.org/ns/dcat#downloadURL": {
        "@id": "https://raw.githubusercontent.com/chrisfinch/over9k/master/public/experiments/march-2013/GBR_adm2.json"
      }
    },
    {
      "@id": "_:b4",
      "@type": "http://www.w3.org/ns/dcat#Distribution",
      "http://purl.org/dc/terms/title": {
        "@language": "en",
        "@value": "Grid as CoverageJSON file"
      },
      "http://www.w3.org/ns/dcat#downloadURL": {
        "@id": "http://reading-escience-centre.github.io/leaflet-coverage/coverages/grid.covjson"
      },
      "http://www.w3.org/ns/dcat#mediaType": "application/prs.coverage+json"
    },
    {
      "@id": "http://melodiesproject.eu/datasets/wp2/test",
      "@type": "http://www.w3.org/ns/dcat#Dataset",
      "http://purl.org/dc/terms/description": {
        "@language": "en",
        "@value": "This test dataset includes distributions of various formats"
      },
      "http://purl.org/dc/terms/issued": {
        "@type": "http://www.w3.org/2001/XMLSchema#dateTime",
        "@value": "2015-10-13"
      },
      "http://purl.org/dc/terms/spatial": {
        "@id": "_:b1"
      },
      "http://purl.org/dc/terms/temporal": {
        "@id": "_:b2"
      },
      "http://purl.org/dc/terms/title": [
        {
          "@language": "de",
          "@value": "Test Datensatz zum Testen von Distributionen"
        },
        {
          "@language": "en",
          "@value": "Test Dataset for testing distributions"
        }
      ],
      "http://www.w3.org/ns/dcat#distribution": [
        {
          "@id": "_:b3"
        },
        {
          "@id": "_:b4"
        }
      ],
      "http://www.w3.org/ns/dcat#landingPage": {
        "@id": "http://melodiesproject.eu/datasets/wp2/test"
      }
    }
  ]
}
```

### Output

```js
{
  "@context": [
    "https://rawgit.com/ec-melodies/wp02-dcat/master/context.jsonld",
    {
      "@base": null,
      "@language": null,
      "title": {
        "@id": "dct:title"
      },
      "description": {
        "@id": "dct:description"
      },
      "title_i18n": {
        "@id": "dct:title",
        "@container": "@language"
      },
      "description_i18n": {
        "@id": "dct:description",
        "@container": "@language"
      }
    }
  ],
  "@id": "_:b0",
  "@type": "Catalog",
  "title_i18n": {
    "en": "Test Datasets"
  },
  "datasets": [
    {
      "@id": "http://melodiesproject.eu/datasets/wp2/test",
      "@type": "Dataset",
      "description_i18n": {
        "en": "This test dataset includes distributions of various formats"
      },
      "issued": "2015-10-13",
      "spatial": {
        "@id": "_:b1",
        "@type": "Location",
        "geometry": "POLYGON((-180 -90, 180 -90, 180 90, -180 90, -180 -90))"
      },
      "temporal": {
        "@id": "_:b2",
        "@type": "PeriodOfTime",
        "endDate": "2015-12-31",
        "startDate": "2015-01-01"
      },
      "title_i18n": {
        "de": "Test Datensatz zum Testen von Distributionen",
        "en": "Test Dataset for testing distributions"
      },
      "distributions": [
        {
          "@id": "_:b3",
          "@type": "Distribution",
          "format": "geojson",
          "title_i18n": {
            "en": "UK Admin 2 Boundaries as GeoJSON"
          },
          "downloadURL": "https://raw.githubusercontent.com/chrisfinch/over9k/master/public/experiments/march-2013/GBR_adm2.json"
        },
        {
          "@id": "_:b4",
          "@type": "Distribution",
          "title_i18n": {
            "en": "Grid as CoverageJSON file"
          },
          "downloadURL": "http://reading-escience-centre.github.io/leaflet-coverage/coverages/grid.covjson",
          "mediaType": "application/prs.coverage+json"
        }
      ],
      "landingPage": "http://melodiesproject.eu/datasets/wp2/test"
    }
  ]
}
```