# Converting Annotated Research Data from Excel to JSON-LD Using R
The provided R code can be used and adapted to convert annotated research data stored as .xlsx file into a JSON-LD file. The provided code was developed to promote FAIRification of legacy datasets that only have limited information on metadata available. However, the code in its current version mainly served as a prove of concept for one specific dataset. Hence, adjustments have to be made, to adapt the code for datasets other than the provided example and users are advised to check the resulting JSON-LD files. 

## Requirements:
### 1. R Environment and Libraries
- **R Installation:** Ensure R is installed on your system.
- **Required R Packages:**
  - `readxl`
  - `jsonlite`
  - `lubridate`
  - `dplyr`
### 2. Excel File Structure

- **File Location:** The Excel file must be at the path specified in the `EXCEL_FILE` variable, and the script must have read permissions.

- **Required Sheets:**
  - `measurements` – Contains measurement data.
  - `entity_metadata` – Contains metadata about the entities (e.g., plots).
  - `ontology_map` – Maps prefixes to their ontology base URIs.

- **Expected Columns (Case-Sensitive):**

#### `measurements` Sheet

| Column Name       | Description |
|-------------------|-------------|
| `entity_id`       | Unique ID linking to an entity in the metadata. |
| `trait_label`     | Human-readable label of the trait. |
| `trait_ontology`  | Ontology URI of the trait. |
| `value`           | Measured value (attempts numeric conversion). |
| `unit_ontology` *(optional)* | Ontology URI for unit. |
| `unit` *(optional)*          | Textual unit (e.g., "cm"). |
| `date` *(optional)*          | Date of measurement (multiple formats supported). |
| `type`            | Measurement type (e.g., "schema:QuantitativeValue"). |

#### `entity_metadata` Sheet

| Column Name       | Description |
|-------------------|-------------|
| `entity_id`       | Unique ID for each entity (e.g., plot ID). Must match `entity_id` in `measurements`. |
| `entity_type`     | RDF type of the entity (e.g., "schema:Plot"). |
| `attribute`       | Attribute/property (e.g., "schema:name"). |
| `value`           | Value of the attribute. |
| `value_type` *(optional)* | Data type (e.g., "@id", "decimal", "integer"). Defaults to string. |

#### `ontology_map` Sheet

| Column Name | Description |
|-------------|-------------|
| `prefix`    | Prefix used in the dataset (e.g., "schema"). |
| `uri_base`  | Base URI for the prefix (e.g., https://schema.org/). |

---

## Configuration Variables

Ensure the following variables are correctly set in the R script:

- `EXCEL_FILE`: Path to your Excel file.
- `MEASUREMENTS_SHEET`: Sheet name for measurements.
- `METADATA_SHEET`: Sheet name for entity metadata.
- `ONTOLOGY_MAP_SHEET`: Sheet name for ontology mappings.
- `OUTPUT_JSONLD_FILE`: Output filename (e.g., `output_final.jsonld`).
- `ENTITY_BASE_URI`: Base URI for entities (e.g., `https://your.data.instance.org/entity/`).

---

## Data Consistency Checklist

- **Entity Linking:** `entity_id` must be consistent between `measurements` and `entity_metadata`.
- **Ontology Prefixes:** Prefixes (e.g., `schema:`) used in data must be defined in the `ontology_map` sheet.
- **Date Formats:** Use consistent and parseable date formats:
  - Recommended: `YYYY-MM-DD`, `DD-MM-YYYY`, `MM-DD-YYYY`, or Excel date serial numbers.

## Final Note
By adhering to these requirements, you should be able to run the provided R code and generate a JSON-LD file representing your data with the specified structure and semantic annotations. Remember to check the console output for any warnings or errors during the data loading and processing steps.

## Acknowledgements
The **HortSEEDS Project** was conducted as FAIRagro Pilot Use Case and was funded by the German Research Foundation (DFG) - project number 501899475.
