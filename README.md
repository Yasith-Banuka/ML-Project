https://github.com/Yasith-Banuka/ML-Project
<br/><br/>
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

* Following 7 sets of features refer to the same detail, and is identical in most rows. The cardinality is slightly different in each feature
    * Scheme_management | management | management_group
    * Extraction_type | extraction_type_group | extraction_type_class
    * Payment | payment_type
    * Water_quality | quality_group
    * Quantity | quantity_group
    * Source | source_type | source_class
    * Waterpoint_type | waterpoint_type_group
<br/><br/>

__Correlation__
<br/><br/>

![alt text](https://github.com/Yasith-Banuka/ML-Project/blob/main/Images/Correlation.JPG)

As expected, all the location features are correlated. So are the 7 sets of similar features.
<br/><br/>

## Feature Selection<br/><br/>

Following features were selected based on initial analysis

* amount_tsh
* funder
* longitude
* latitude
* Basin
* Subvillage
* Region_code
* district_code
* lga
* ward
* population
* public_meeting
* permit
* extraction_type_group
* payment
* quality_group
* quantity
* source_class
* waterpoint_type_group
* management
* source_management
* construction_year

Following features were removed

* date_recorded - no useful information
* installer - similar to funder
* wpt_name - High cardinality and missing values
* num_private - no information
* region - similar to region_code
* recorded_by - no information
* scheme_name - missing values

One feature from each of the 7 groups of features mentioned in the earlier section was picked considering cardinality and target rellevence.

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
* removed all erronous entries containing 0
* Created a new feature called 'age'
* The age of the record in 2013 is recorded in this feature (age = 2013 - construction_year.year)

__Permit | Public meeting__
* Encoded with 0 and 1 for False and True respectively.
<br/><br/>
## Handling missing values<br/><br/>

Data type | Columns | Method
------------ | -------------   | -----
Numerical | age<br/> longitude<br/>gps_height|mean
Boolean | public_meeting<br/>permit | mode
Categorical | management_cross<br/>subvillage | 'nan' category

<br/><br/>
## Catboost<br/><br/>

Multiple algorithms were tested for the problem, and catboost performed best among them. Due to the high number of categorical features in the dataset, catboost is a great algorithm to use (catboost has many features to support categorical features). It also helps with many other aspects in the pipeline.
<br/><br/>

__Encoding__
 
Catboost has its own target encoder which is ideal for classification. Since all the categorical features are nominal and of high cardinality, this is the best method.
<br/><br/>
__Regularization__ 

Catboost performs L2 regularization on its models, which is also optimized in hyperparameter tuning.
<br/><br/>
__Hyperparameter tuning__

Done using HypoerOpt. The most important parameters were choen to be optimized. Features selected for tuning are:
* learning rate
* depth of tree
* subsampling rate for bagging
* model size regularization
* feature combination
<br/><br/>

__Cross-validation__

A 6-fold cross validation is done using catboost's in-build cross-validation functionality.
<br/><br/><br/>


## Post-Processing
<br/><br/>

__Feature Importance__
<br/>

feature | feature Importance
--------------|------------------
quantity|35.494263
waterpoint_type_group|22.619132
ward|19.299193
lga|15.545347
extraction_type_group|3.930602
payment|2.698304
age|0.207756
funder|0.205402
source_class|0.000000
quality_group|0.000000
amount_tsh|0.000000
public_meeting|0.000000
district_code|0.000000
basin|0.000000
latitude|0.000000
longitude|0.000000
gps_height|0.000000
population|0.000000
<br/><br/>

__SHAP Values__
<br/><br/>

![alt text](https://github.com/Yasith-Banuka/ML-Project/blob/main/Images/shap.JPG)

<br/><br/>
Following features were found to be less important by the above diagrams, and thus removed from training

* population
* gps_height
* longitude
* latitude
* basin
* subvillage
* region_code
* permit
* public_meeting
* district_code

<br/><br/>

## Competition submission
<br/><br/>
![alt text](https://github.com/Yasith-Banuka/ML-Project/blob/main/Images/submission.png)

<br/><br/>

## References

Following resources were used in building this solution.
<br/><br/>

https://towardsdatascience.com/pump-it-up-with-catboost-828bf9eaac68

https://github.com/catboost/tutorials



 


 
