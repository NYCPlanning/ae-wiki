# Description
Standardize the naming of methods, functions, and OpenAPI operations around some shared conventions.

1. REST-associated names start with a standardized verb associated with their http method.
   - Get -> `find`
   - Post -> `create`
   - Patch -> `update`
   - Put -> `replace`
   - Delete -> `remove`
2. Domain names are included based on whether it is used in its own domain (Domain as in `tax lot` or `borough`)
   - Within controllers, services, and repos domain names are excluded when the method is within that domain.
     - Domain names used outside of their home domain are stated explicitly
     - For example, within the zoning district domain the method to get a zoning district would be `findById`
     - Within the tax lot domain, the methods to get the zoning districts associated with a tax lot would be `findZoningDistrictsByBbl`
   - For operationIds, the domain is included because its variables are used at a global scope
     - The endpoint to get a zoning district by its id would be `findZoningDistrictById`.
     - The endpoint to get zoning districts associated with a tax lot would be `findZoningDistrictsByTaxLotBbl`
  - The domain rule applies to path parameters, as well.
    -  The path parameter for tax lot bbls would be `bbl` when used within the tax lot domain but it would be `taxLotBbl` in the OpenAPI documenation
3. Lists of results are indicated either with plurality or the literal word 'Many'
    - Plurality is used when writing domain names outside of their home domain
      - ie) `findZoningDistrictsByBbl`  when finding a list of zoning districts in the tax lot domain
    - "Many" is used when referring to the home domain (the domain name is omitted and we would like an alternate indicator)
      - 'Many' is preferred to 'All', because it's general enough to allow for filters/pagination and still make sense
      - ie) `findMany` when  finding a list of boroughs in the borough domain
4. Classes are singular
    - Classes represent a template from which several instances may be created. They can be modeled more easily as singular.
    - File names are also singular, as they follow the name of the class
5. Variable names with corollary table columns should reflect the name of the column
   - `zoningDistrictId` v. `zoningDistrictUuid` has caused a bit of confusion. `ZoningDistrictId` should be used because the name of the column is `id`. `uuid` is merely the current format; it does not reflect the meaning of the variable.
   - `taxLotBbl` is fine because `bbl` is the column name for the primary id of tax lots.
   - To reinforce point 2, the variable names should include the domain name when used in a global context. This includes OpenApi documentation. The variable names should exclude the domain name when used in its home domain.
6. Non-default formats should be affixed to the "object" in the variable name
    - If getting the geojson of zoning districts by a tax lot bbl, it should be `findZoningDistrictsGeoJsonByBbl`
    - If getting the geojson format of a tax lot within the tax lot domain, the domain is excluded like so: `findGeoJsonByBbl`
    - If getting a list of geojson within the domains home, the object is "Many", like so: `findManyGeoJson`
    - The same rule would apply if we started supporting formats like `csv`
7. Initialisms that are subject to camelCase only capitalize the first letter.
   - ie) defaultUrl

## Historical discussion

Originating discussion: [ae-zoning-api #115](https://github.com/NYCPlanning/ae-zoning-api/discussions/115)
