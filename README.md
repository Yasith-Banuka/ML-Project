# ML Project - "Pump-It-up" <br/>

## Exploratory Analysis<br/><br/>
Upon performing an analysis on the dataset, the following characteristics on the features were found.

* Amount_tsh - Contains a large number of zeroes. Erronous, actual data, or a combination.

* Wpt-name - Extremely high cardinality. 

* Num_private - 98% zeroes. 

* recorded by - same value throughout

* Funder | installer - Entries are common in most records. Many errornous entries resulting in high cardinality. Many missing values

* Basin | subvillage | region | region_code | district_code | longitude | latitude - Location-based features. Some are of high cardinality. 
    * Longitude - has errornous zeroes.

* gps_height - contains negative entries (which is impossible i.e. errornous data). Also contains many zeroes

* Following sets of features refer to the same detail, and is identical in most rows. The cardinality is slightly different in each feature
    * Scheme_management | management | management_group
    * Extraction_type | extraction_type_group | extraction_type_class
    * Payment | payment_type
    * Water_quality | quality_group
    * Quantity | quantity_group
    * Source | source_type | source_class
    * Waterpoint_type | waterpoint_type_group
<br/><br/>

## Feature Engineering<br/><br/>

__Funder__

As funder & installer are similar, fuder was chosen and installer was dropped. An extensive cleaning was performed. 
* lowercase all entries to eliminate any case-based errors.
* Fill any mising data available from installer
* Fill all remaining missing data  and invalid data by 'nan'
* Look for the common words in the columns and combine all entries containing those words into a single columns
    * E.g. Combine all entries containing 'unicef' under a single category as 'unicef'
* Combine all similar funders into one category
    * E.g. All governmental funders into one category
* Replace all categories with less than 100 entries into 'other' category

__Management__

Interesting nuances between scheme_management & management was observed, thus a cross feature called 'management_cross' was created. Nost of the entries remained as is, but some new categories were created. Any category with less than 100 entries was put into a 'other' category. Missing values filled with a 'nan' category.

__gps_height__

* Get absolute values to eliminate negative readings


__Construction year__

* Created a new feature called 'age'
* The age of the entry in 2013 is recorded in this feature

__Permit | Public meeting__
* Encoded with boolean 0 and 1
<br/><br/>
## Handling missing values<br/><br/>

Data type | Columns | Method
------------ | -------------   | -----
Numerical | age<br/> longitude<br/>gps_height|mean
Boolean | public_meeting<br/>permit | mode
Categorical | management_cross<br/>subvillage | 'nan' category




 
