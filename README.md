### 15-digit-to-18-digit-IDs-Salesforce
Remedy to getting 18 digit IDs from Salesforce - Required for data export out of SF to any other system.

1.  For each object you need to export, you need to create a formula field that uses the SAFE Id.
2.  Create a custom field:
  - Formula field
  - Result: Text
  - Formula = CASESAFEID (Id)
