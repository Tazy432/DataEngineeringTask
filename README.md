Data Cleaning and Merging for Facebook, Google, and Website Datasets
Overview
This project involves cleaning and merging three datasets: facebook_dataset, google_dataset, and website_dataset. The datasets contain overlapping information, primarily keyed on phone numbers, with additional fields such as country codes, emails, addresses, and business details.

Data Cleaning
facebook_cleaner:
Fix Text Encoding: Applied ftfy.fix_text to all columns to ensure correct decoding.
Spaces Normalization: Replaced multiple spaces with single spaces.
Country Code and ISO-2 Alpha Code Retrieval: Scraped data from a site to retrieve phone country codes and ISO-2 alpha codes for each country. Mapped country names and codes interchangeably.
Phone Formatting:
Ensured phone numbers have sufficient digits.
Converted numbers from scientific notation into standard phone numbers.
Removed invalid numbers (those not matching the country code or incorrect length) and marked them as "WRONG."
Email Validation: Checked email addresses with a regex. Invalid emails were set to "WRONG."
Link Validation: Used regex to validate URLs. Invalid links were set to "WRONG."
Address Parsing:
Identified elements such as zip codes, country names, region codes, etc., from the address column.
Moved these elements to their respective columns (e.g., region name, region code) using a chunking process for efficiency.
The city name is left in the address column for further identification.
Save Cleaned Data: Saved the cleaned data to facebookFINAL.csv.
google_cleaner:
Error Handling: Read the dataset using the csv module, ignoring erroneous lines and processing it in chunks.
Country Code Mapping: Used scraped data to map between country names and codes.
Address Parsing:
Split the address column and compared its contents with other columns (e.g., phone, region name, country code).
Removed redundant information (e.g., country names or region codes inside the address column).
Compared saved values exactly against columns like region_code and country_code to avoid unintentional data modification.
Phone Number Formatting: Corrected phone numbers from scientific notation and checked for invalid length, marking incorrect ones as "WRONG."
Business Details Extraction:
Extracted details such as rating, number of reviews, and years of experience from complex strings in the text column.
Created new columns for these fields and cleaned unnecessary text.
Save Cleaned Data: Saved and concatenated chunked data into google.csv.
website_cleaner:
Phone Number Formatting: Converted phone numbers from scientific notation into standard phone numbers (using regex \+\d{11}) and validated their lengths.
Save Cleaned Data: Wrote the cleaned data to website.csv.
Data Merging
Steps:
Website and Google Merge:

Merged website.csv with google.csv on the phone column using a left join, prioritizing website data.
Renamed columns with conflicts, and filled in missing values using data from the corresponding _y column.
Dropped unnecessary columns.
Facebook Merge (In Progress):

Plan to merge the result with facebookFINAL.csv on the phone column.
Conflict resolution and further column cleanup are pending.
Notes
The merging process is still being refined, especially around handling merge conflicts and ensuring accurate data integration across datasets.
Future steps include improving the city identification and resolving conflicts during the final merge with facebook_dataset.
