# Reigman Real Estate Analysis

## 1.Business Understanding

Reigman Real Estate is a real estate company situated in King County and they primarily deal in buying and selling of properties.In order to make well informed decisions on what type of house should be sold or bought, the company has sought out a data scientist to help them make profitable decisions based off the available data on housing sales in the region.The aim is to find which factors influence the pricing of houses and by how much

## 2.Data Understanding

The data available to us is the King County house sales dataset which contains columns that are different properties related to a house.The dataset has a data dictionary which has a breif description of column content:

### Column Names and Descriptions for King County Data Set
* `id` - Unique identifier for a house
* `date` - Date house was sold
* `price` - Sale price (prediction target)
* `bedrooms` - Number of bedrooms
* `bathrooms` - Number of bathrooms
* `sqft_living` - Square footage of living space in the home
* `sqft_lot` - Square footage of the lot
* `floors` - Number of floors (levels) in house
* `waterfront` - Whether the house is on a waterfront
  * Includes Duwamish, Elliott Bay, Puget Sound, Lake Union, Ship Canal, Lake Washington, Lake Sammamish, other lake, and river/slough waterfronts
* `view` - Quality of view from house
  * Includes views of Mt. Rainier, Olympics, Cascades, Territorial, Seattle Skyline, Puget Sound, Lake Washington, Lake Sammamish, small lake / river / creek, and other
* `condition` - How good the overall condition of the house is. Related to maintenance of house.
  * See the [King County Assessor Website](https://info.kingcounty.gov/assessor/esales/Glossary.aspx?type=r) for further explanation of each condition code
* `grade` - Overall grade of the house. Related to the construction and design of the house.
  * See the [King County Assessor Website](https://info.kingcounty.gov/assessor/esales/Glossary.aspx?type=r) for further explanation of each building grade code
* `sqft_above` - Square footage of house apart from basement
* `sqft_basement` - Square footage of the basement
* `yr_built` - Year when house was built
* `yr_renovated` - Year when house was renovated
* `zipcode` - ZIP Code used by the United States Postal Service
* `lat` - Latitude coordinate
* `long` - Longitude coordinate
* `sqft_living15` - The square footage of interior housing living space for the nearest 15 neighbors
* `sqft_lot15` - The square footage of the land lots of the nearest 15 neighbors

## 3.Modeling

The purpose of modeling is to enable us to predict values based off the available data.The column we are most intrested in is price and we would like to see how the other features of a house affect it.
The correlation of these features to price can be show via scatterplots

![Scatterplots]
("data\Scatter plots.png")

Once you confirm the correlation we take a look at the distribution of the columns

![kde]
("data\kde.png")

From the above visualisation we see that the columns do not have a normal distribution and that they are skewed.This skewness indicates presence of outliers in the columns and these can reduce the performance of the model
The presence of outliers will make our model less precise but not biased if you are using a linear model 
The first model is a linear regression which uses thge house price as  the dependant variable and sqft_living as the independent variable to see its influence on price

The R-squared of the model is 0.49 meanig 49% of the variance in price and it is statistically significant and the models prediction is off by $173824

The constant and sqft_living coefficient are about -$43990  and 281 respectively and they are both statistically significant.Hence the formula to predict price based off of sqft_livin is
###         price = 281*sqft_living - 43990

Below we visualize the results

![predicted]
("data\Actual vs Predicted.png")

![qqplot]
("data\qqplot.png")

The ggplot shows us that it would have been best suited to make a polynomial regression for the price and sqft_living since more variance would have been covered

We are now intrested in making a multilinear model for all the other features we are intrested in and see how they affect the price.This can be made possible by the first linear regression which will become the baseline.The columns of intrest are `sqft_living`, `sqft_living15` and `grade`

The overall model is statistically significant having an adjusted r-squared of about 60 meaning that 60% of the variance in price is covered.In predictions the model is of by $156659.

From the constant and coefficients of the column the price can be predicted by multiplying the associate value from a column with its coefficient getting the sum of all the features and their associated values and adding the constant


## 4.Regression Results

Comparing the two models the multilinear model did better beacause it covers about 60% of the variance in price while the baseline model covers about 50% .Also the multilinear models predictions are off by $156659 and baseline model is off by $173824.Our recommendations will be based off the multilinear model

All coefficients in the multilinear model are statistically significant apart from grade_3 Poor and grade_4 Low 

* The intercept is at about $121700. This means that a house with a
  `grade` of 7(average) would sell for $121700.
* The coefficient for `sqft_living` is about $152. This means for each additional squared foot increase,
  the house costs about $152 more.
* The coefficient for `sqft_living15` is about $14. This means for each additional squared foot increase for the nearest 15 neighbours,
  the house costs about $173 more.
* The coefficients for `grade` range from about $2395000 to about -$42500
  * For a grade of "5(fair)" compared to a grade of "7(average)", we expect -$42500 price
  * For a grade of "6(low average)" compared to a grade of "7(average)", we expect -$20860 price
  * For a grade of "8(good)" compared to a grade of "7(average)", we expect +$59560 price
  * For a grade of "9(better)" compared to a grade of "7(average)", we expect +$178600 price
  * For a grade of "10(very good)" compared to a grade of "7(average)", we expect +$372100 price
  * For a grade of "11(excellent)" compared to a grade of "7(average)", we expect +$657500 price
  * For a grade of "12(luxury)" compared to a grade of "7(average)", we expect +$1192000 price
  * For a grade of "13(mansion)" compared to a grade of "7(average)", we expect +$2395000 price

## 5.Conclusion

The findings from the modellimhg can lead us to conclude:
   * The bigger the houses living space the higher the price
   * The bigger the living spaces of the neighbouring 15 houses the higher the price of the house
   * The price of houses with a grade below 7(average) compared to the price of houses with a grade of 7 is lower
   * The price of houses with a grade above 7(average) compared to the price of houses with a grade of 7 is higher



