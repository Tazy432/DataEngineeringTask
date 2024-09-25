
###Instructions for Running the App
Download the datasets: Google Drive Link
Make sure you have Git installed.
Open the command prompt in the folder where you have downloaded the datasets.
Type the following command:
git clone "https://github.com/Tazy432/DataEngineeringTask"
Install all the required modules and packages.

The order in which you run the project matters:
First: Run facebook_cleaner.ipynb
Second: Run google_cleaner.ipynb
Third: Run website_cleaner.ipynb
Fourth: Run finalMerge.ipynb
Data Cleaning Process
###facebook_cleaner:
We start by applying ftfy.fix_text for the columns, to ensure clean decoding.
We transform all multiple spaces into single spaces.
We scrape data from a site to retrieve phone country codes and ISO-2 alpha codes for each country.
If we have the code of the country, then we have the name of the country. If we have the name of the country, we have the code (using the scraped data).
We format phones (we make sure they have enough digits, and we transform the phone numbers from scientific notation into normal phone numbers).
We get rid of all wrong numbers (a wrong number is one that doesn’t start with the phone code of its country, or if the number of digits is not 11). We place "WRONG" instead of the number if it’s wrong.
We then check the email records using a regex. If the email doesn’t have the correct form, we set it to "WRONG."
We check the link records using a regex. If the link doesn’t have the correct form, we set it to "WRONG."
It is easy to notice that inside the address column we have many values that look like zip code, country name, country code, region name, region code, etc. Using the module pycountry and the zipRegex (which contains the patterns for zip codes for all countries), if we have the region name in the address, then we delete the region name from the address and add it into the region name column. We use chunk_size for this operation since it’s more complex.
This way, the only value still left behind is the city, since I haven’t yet found a way to check if a string is indeed the city name of a given country.
We save the data to facebookFINAL.csv.
###google_cleaner:
We read the messy file using the csv module to ignore the error lines.
We read it using chunksize due to the size of the dataset.
If we have the code of the country, then we have the name of the country. If we have the name of the country, we have the code (using the scraped data).
We see that in the address column we have values from other columns (country name in address, phone number, region name, etc.). We split the address by commas and search to see if these elements exist under other columns as well.
If they exist, then we can remove them (we save only the values that are not in the text column, phone column, region name column, city column, etc.). If not, we search to see if the elements contain the zip code (as many instances do).
We compare the saved values against certain columns such as region_code, country_code (we compare them exactly, checking if they’re equal, unlike before when we checked if a string exists in another string). This helps avoid problems like removing needed data (e.g., "ca" from "calendar" turning into "nlendar").
We check for scientific notations in phone numbers and change them to simple phone numbers or mark them as "WRONG" if the number of digits is incorrect.
We upload the data regarding country phone numbers into the country_phone_number column.
Inside text, we have many instances of strings such as:
"4.3 (15) Â· Medical spa 7+ years in business Â· 11040 Santa Monica Blvd Suite 370 Closed â‹… Opens 9AM Tue Â· +1 818-426-8353".
We transform this column into four actual columns:
Number of reviews: 15,
Rating: 4.3,
Experience: 7 years.
We then erase unnecessary data, resulting in the text now being:
"· Medical spa · 11040 Santa Monica Blvd Suite 370 Closed â‹… Opens 9AM Tue Â· +1 818-426-8353"
We save the data from each chunk and concatenate it together into google.csv.
###website_cleaner:
We read the data using pandas.
We transform the phone numbers from scientific notation into basic phone numbers (e.g., \+\d{11}), and then we check the number of digits to ensure the phone number is correct.
We save the data and write it to a new file called website.csv.
###Merging:
Not complete - many things need attention:
We merge the data from the website with the data from Google on the phone number (left merge, we are especially interested in website data since it is 5 times smaller than the Google dataset).
We merge on phone (phone is a pretty unique string, so we might use it as the key).
We rename some columns to avoid conflicts, then solve them by filling in missing values. For example:
If A_x is empty and A_y is not empty, set A_x = A_y, and vice versa.
We drop some unnecessary columns.
Not working right now:
We merge with Facebook data on the phone.
