# Housefacts Data Specification

## Format and Frequency

* All files in an OHHS feed should be consolidated in a single ZIP file - YYYYMMDD_name.zip
* All files in an OHHS feed must be saved as comma-delimited text, except the optional shapefile of parcels/lots with unique identifiers.
* The first line of each file must contain field names. Field names for each file are defined below under the ‘Files’ heading.
* All field names are case-sensitive.
* Field values may not contain tabs, carriage returns or new lines.
* Field values that categorize by type should contain a finite and standardized list of category descriptors with identical spelling, punctuation, capitalization and phrasing for all values in the same category.
* Field values that contain quotation marks or commas must be enclosed within quotation marks. In addition, each quotation mark in the field value must be preceded with a quotation mark. This is consistent with the manner in which Microsoft Excel outputs comma-delimited (CSV) files. For more information on the CSV file format, see http://tools.ietf.org/html/rfc4180.
* The following example demonstrates how a field value would appear in a comma-delimited file:
* Original field value: Contains "quotes", commas and text
* Field value in CSV file: "Contains ""quotes"", commas and text"
* Field values must not contain HTML tags, comments, escape sequences, or anything else designed to break or confuse our computers.
* Remove any extra spaces between fields or field names. Many parsers consider the spaces to be part of the value, which may cause errors.
* Each line must end with a CRLF or LF linebreak character.
* Files should be encoded in UTF-8 to support all Unicode characters. Files that include the Unicode byte-order mark (BOM) character are acceptable.
* Files must be provided in a single .zip archive with flat hierarchy.
* Files should be updated weekly to reflect current inspection results.

## Files
All required CSV files and any available optional files must be collected in a single ZIP file.

### buildings.csv
The building.csv file contains information about the residential building, such as location, owner, age, and value. The building_id is a unique identifier defined by the locality for the building - a contiguous structure with one or more residential dwelling units.  

File Required

| Name |  Type | Required | Description |
|:-----|:------|:---------|:------------|
| building_id | string | Yes | Building_id is a unique alpha-numeric identifier created or assigned by a single authoritative agency in a locale (e.g. tax collector)  that identifies each residential building inspected by any agency in locality. See source and definition of building_id in feed.csv below. |
| parcel_id | string | no | Unique BLKLOT identifier |
| building_type | string | no | Type of residential property ( single family, duplex, multi-family, single room occupancy,  etc) |
| dwelling_units | number | yes | Number of residential dwelling units |
| building_built_year | number | no | Year building was constructed or rebuilt. |
| building_sqft | number | No | Recorded building square feet | 
| building_value | number | No | Current assessed property value. |
| building_from_st | string | Yes | Lowest numerical value of the street number if the building has an address range or only building street number. |
| building_to_st | string | No | Highest numerical value of the street number if the building has an address range. |
| building_street | string | Yes | Street name |
| building_street_type | string | Yes | Street type (St, Ave, Way) |
| building_city | string | Yes | City where the property is located. |
| building_state | string | Yes | State where the property is located. In the U.S. this should be the two-letter code for the state
| building_postal_code | string | Yes | Zip code or other postal code |
| building_latitude | number | Yes | Latitude of the residential building parcel. This field must be a valid WGS 84 latitude. For example 37.7859547
| building_longitude | number | Yes | Longitude of the residential building parcel. This field must be a valid WGS 84 longitude. For example -122.4024658
| owner_name | string | Yes | Property owner. This may be an individual or business entity. | 
| owner_mailing_address | string | No | Mailing address for the owner of record. For example Jane Smith 310 Polaris Way San  Francisco CA 94117 |

### inspections.csv
The inspections.csv file contains information about regulatory inspections of residential buildings that have occurred over the past five years.  Where regulation is the responsibility of multiple local agencies, a city may provide all inspections in one file or each agency  may provide separate file.  Inspections may be either reactive (e.g., a complaint response) or pro-active (e.g. routine or scheduled). 

File required

| Name |  Type | Required | Description |
|:-----|:------|:---------|:------------|
| building_id | string | Yes | Unique identifier for the residential property being inspected. |
| parcel_id | string | No |
| inspection_id | string | Yes | Unique ID for inspection; format provided by municipality |
| inspection_address | string | No |Street address of the residential building. For example 706 Mission St. Format should conform to ESRI government data scheme for address. |
| inspection_unit_number | string | No | Required if if inspection is for specific unit in a multi-unit building. A unit name or number is acceptable.
| inspection_city | string | No | City where the property is located. |
| inspection_state | string | No | State where the property is located. In the U.S. this should be the two-letter code for the state. |
| inspection_agency | string | Yes | String representing the inspecting agency.  Must be one of the following types: Health, Environment, Building, Housing, Planning, Fire, Code Enforcement.|
| agency_jurisdiction | string | Yes | Must be one of the following: city, county, state, federal. |
| inspection_type | string | Yes | String representing the type of inspection. Must be one of the following types: permit, routine, complaint, follow-up. |
| inspection_date | date | Yes | Date of the inspection in YYYY-MM-DD format |
| inspection_rating | string | Yes | Summary inspection rating or result (e.g. pass/ fail, satisfactory/ not-satisfactory, etc) associated with the inspection if one exists.|
| inspection_score | number | No | Optional. Numerical score associated with the inspection if one exists |

## violations.csv

The violations.csv file contains information about specific violations. 

This file is required.

| Name |  Type | Required | Description |
|:-----|:------|:---------|:------------|
| violation_id | number | Yes | Unique identifier for the violations |
| inspection_id | | number | Yes | Unique identifier for the inspection |
| inspection_agency | string | Yes | String representing the inspecting agency.  Must be one of the following types: Health, Environment, Building, Housing, Planning, Fire, Code Enforcement |
| violation_address | string | No | Street address of the residential building. For example 706 Mission St. Format should conform to ESRI government data scheme for address. |
| violation_unit_number | string | No | Required if if inspection is for specific unit in a multi-unit building. A unit name or number is acceptable. |
| violation_city | string | No | City where the property is located. |
| violation_state | string | No | State where the property is located. In the U.S. this should be the two-letter code for the state.|
| violation_date | date | Yes | Date that the  violation was recorded in YYYY-MM-DD format. This should correspond with at least one inspection date. |
| violation_date_closed | date | Yes | Date that the  violation was closed in YYYY-MM-DD format. |
| violation_category | string | Yes | Reporting agency must assign each locally defined violation type on one of the following categories in the OHHS list of violation categories |
| violation_type | string | Yes | Locally defined violation type (bedbugs, mold, etc) |
| violation_severity | string | No | Must be one of the following three categories: high (imminently harmful to health and requires rapid correction) and low (nuisance with correction over a reasonable period of time) |
| legal_authority | string | No | The legal authority under which a violation is issued.  For example, NYC Housing Maintenance Code or the California Health and Safety Code. |

## feed_info.csv
The feed.csv file contains information about the source of the data in the files. This file should only contain a single row. 

This file is required.


| Name |  Type | Required | Description |
|:-----|:------|:---------|:------------|
| feed_date | date | Yes | Date this feed was generated in YYYY-MM-DD format | 
| feed_version | string | Yes | Version of the OHHS specification used to generate this feed. For example ‘0.1’
| municipality_name | string | Yes | Name of the municipality providing this feed. For example ‘San Francisco’ or ‘Multnomah County’|
| building_id_definition | string | Yes | Describes how the municipality defines or creates the variable building_id convention for defining  building_id. |
| parcel_id_definition | string | Yes | Describes how the municipality defines or creates the variable parcel_id |
| municipality_url | string | No | URL of the publishing municipality’s website | 
| contact_email | string | No | Email address of the person to contact regarding invalid data in this feed |

## parcels.zip
The parcels shapefile contains municipality-wide information about the parcel or lot property lines. The shapefile will be comprised of six files with the same name ending in .dbf, .prj, .sbn, .sbx, .shp, .shx, and .shp.xml. This file can be provided separately from the .csv files as it may be updated on a different frequency.

This file is optional

| Name |  Type | Required | Description |
|:-----|:------|:---------|:------------|
| Shape | geometry | Yes | This field contains data that describes the boundaries of the lot. It is generated automatically when the shapefile is created and will display a type of polygon or multipolygon.|
| parcel_id | string | Yes | Local identifier for all parcel footprint that the building sits on. |
| building_id | string | Yes | Unique identifier for the residential building.  Can correspond to more than one parcel_id or an address range |


## Violation Categories
The table below provides a lookup table of standard violation categories. This is lookup table is used to populate the violation categories field in violations.csv.

| Violation Category | Violation Category Examples |
|:-------------------|:----------------------------|
| Animals and Pests | Cockroaches; rats; mice; bedbugs; racoons |
| Vegetation | Overgrown vegetation, landslide |
| Refuse | Refuse accumulation; dumping; spoiled food |
| Sanitation | Unsanitary floors or walls; non-functioning sewage system |
| Radiation Hazard | Radon |
| Air Pollutants and Odors | Nuisance odors; smoking in common areas |
| Bioological Hazard | Medical waste; Contaminated Needles, mold/mildew |
| Chemical Hazards | Lead hazard; asbestos hazard |
| Noise | Interior noise violation; exterior noise violation | 
| Indoor Climate | Inadequate heat or ventilation |
| Plumbing | No running water; inoperable toilet |
| Electrical | Exposed electrical hazards; excess electrical loads |
| Fire Hazards and Prevention | Non-functioning smoke detector; heat source near combustible material |
| Building Security | Inadequate exterior lighting |
| Building Structure | Water or moisture intrusion; structural hazard |
| Planning or Zoning | Unpermitted use; violations of conditions of use |
| Abandon, Boarded or Substandard Building | Abandoned Building; Blighted Building; Dilapidated Housing |
| No Permit | No permit for electrical, gas, construction, plumbing ect |
| Water Hazard |
| Storm Water; Water Leak; Sewer Leak |
| Gas Hazard | Gas Leak, Illegal Gas Appliance |
| Other | Other |

