# gopos

[![Publish the Docker image](https://github.com/thepieterdc/gopos/actions/workflows/publish.yml/badge.svg)](https://github.com/thepieterdc/gopos/actions/workflows/publish.yml)

_From Ancient Greek τόπος (tópos, “place”)._

Address and place information service in Go.

---

## Running via docker compose
```shell
docker-compose up --build
```

## Dependencies
This module requires [libpostal](https://github.com/openvenues/libpostal) for address resolving.

## Configuration

The following environment variables are required:

- **GOOGLE_API_KEY:** API key for Google Places.
- **MONGO_URI:** MongoDB connection string. This is used to cache Google Place calls to save costs.

## Features

### Format a given address.

**Example request:**

```http request
GET /format?input=Apple%2010955%20N%20Tantau%20Ave,%20Cupertino,%20CA%2095014,United%20States
```

**Response:**

```json
{
  "city": "cupertino",
  "country": "united states",
  "house": "apple",
  "house_number": "10955",
  "postcode": "95014",
  "road": "n tantau ave",
  "state": "ca"
}
```

### Lookup a Google Place ID.

The result of each call is written to a database to prevent sending (and therefore paying for) the same request to
Google twice.

**Example request:**

```http request
GET /google/place/ChIJ37HL3ry3t4kRv3YLbdhpWXE
```

**Response:**

```json
{
  "address_components": [
    {
      "long_name": "1600",
      "short_name": "1600",
      "types": [
        "street_number"
      ]
    },
    {
      "long_name": "Pennsylvania Avenue Northwest",
      "short_name": "Pennsylvania Avenue NW",
      "types": [
        "route"
      ]
    },
    {
      "long_name": "Northwest Washington",
      "short_name": "Northwest Washington",
      "types": [
        "neighborhood",
        "political"
      ]
    },
    {
      "long_name": "Washington",
      "short_name": "Washington",
      "types": [
        "locality",
        "political"
      ]
    },
    {
      "long_name": "District of Columbia",
      "short_name": "DC",
      "types": [
        "administrative_area_level_1",
        "political"
      ]
    },
    {
      "long_name": "United States",
      "short_name": "US",
      "types": [
        "country",
        "political"
      ]
    },
    {
      "long_name": "20500",
      "short_name": "20500",
      "types": [
        "postal_code"
      ]
    }
  ],
  "business_status": "OPERATIONAL",
  "formatted_address": "1600 Pennsylvania Avenue NW, Washington, DC 20500, USA",
  "geometry": {
    "location": {
      "lat": 38.8976763,
      "lng": -77.0365298
    }
  },
  "icon": "https://maps.gstatic.com/mapfiles/place_api/icons/v1/png_71/civic_building-71.png",
  "name": "The White House",
  "place_id": "ChIJ37HL3ry3t4kRv3YLbdhpWXE",
  "plus_code": {
    "compound_code": "VXX7+39 Washington, DC, USA",
    "global_code": "87C4VXX7+39"
  },
  "types": [
    "tourist_attraction",
    "point_of_interest",
    "establishment"
  ],
  "url": "https://maps.google.com/?cid=8167675777476425407",
  "utc_offset": -240,
  "vicinity": "1600 Pennsylvania Avenue Northwest, Washington"
}
```

### Given a coordinate pair, find the timezone.

**Example request:**

```http request
GET /timezone?latitude=37.97153995920827&longitude=23.726713776643596
```

**Response:**

```json
{
  "latitude": 37.97153995920827,
  "longitude": 23.726713776643596,
  "timezone": "Europe/Athens"
}
```

## Releasing
This process is automated via GitHub Actions. In order to make a new release, trigger the `Release` workflow.