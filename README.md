# CityBike Lisbon: Location Data Modelling with Python

## Project
This project explores the influence of surrounding businesses on bike availability at Lisbon CityBike stations. Using data from the CityBike and Foursquare APIs, I performed multivariate regression analysis to assess how proximity to bars, cafes, and other venues impacts station usage.

## Goals
- Identify predictor variables that influence bike availability at Lisbon CityBike stations.
- Visualise trends between station-level bike availability and surrounding locations
- Use multivariate regression analysis to determine whether location context meaningfully predicts bike availability.

## Data

### Tech Stack
- Python (pandas, seaborn, matplotlib, statsmodels)
- APIs: CityBike, Foursquare
- PostgreSQL
- Jupyter Notebooks

## Process

### Step 1 
- city_bikes.ipynb

- Acquired CityBike Data from CityBike API
    - Resulting 195 bike station coordinates
- Parsing JSON File data into Dataframes for cleaning
- CityBike API Data **Timestamp = 12:39 AM Sunday, July 27 2025**

### Step 2
- yelp_foursquare_EDA.ipynb

- Acquired location data from **FOURSQAURE** APIs within 1,000metres of Bike Stations 
    - Data parameters included;
        - Location Name
        - Distance From CityBike Station
        - Location Category
        - Rating
        - Popularity
        - Hours
        - Hours Popular
        - Price
    
    - 9,662 Locations within 1,000 metres radii of Bike Stations
        - Grouped by Bike Station, visualisation methods performed for cleaning, aggregation and modelling

- **Issue** Unable to access YELP API without Business Contact
    - **YELP API Step Ignored**


#### Final cleaned dataset:
- **176 Bike Stations**
- **9,662 surrounding locations (filtered)**

#### SQL Database
- Created SQL Database for future access
    - Created, linked and stored data in normalised tables

### Step 3
- Exploratory Data Analysis
    - Conducted visual inspections with seaborn/matplotlib pairplots
        - Observed null or weak visual relationships 
    - Generated Pearson R correlations for proposed linear relationships & variance inflation factor values for Multicollinearity
    - Observed weak linear correlations between x-variables and y-variables 
        - y-variables
            - **free_bikes**, **total_slots**

    - Cleaning Procedure
        - Foursquare API Limitation: 
            - API limit at 50 locations per bike station, which introduces selection bias.
        - Filtered Locations **distance < 298** metres to remove selection bias of 50 location limitations
            - Bike Station Maximum locations = 49
        - Cleaned Quantity : Bike Stations = 176

### Step 4
- Statistical Modelling
    - Mulivariate Regression Models 
        - Dependent Variables
            - Free Bikes
            - Total Bike Slots
        - Full model with all business categories
        - Refined model using only top independent variables
            - bar_count
            - cafe_counts
        - Adjusted R² improved in simplified model

    - Linear Regression Models
        - Independent Variable
            - Average Popularity of locations
        - Dependent Variables
            - Free Bikes
            - Total Bike Slots
        - Strong correlations
            - Statistically Insignificant
            - Maximum of 1.4% explanatory power on the variability of dependent variables

        
        
## Results

### API Quality
- Foursquare’s API returned a **maximum of 50 location results per coordinate query**, which introduced selection bias in areas with dense points of interest.
- To mitigate this, we applied a filter of **< 298 metres**, reducing the max results per Bike Station to **49**.
- Final cleaned dataset includes **176 Bike Stations** with confirmed nearby locations under this threshold.

### Visual Observations
- Pairplots showed **no clear visual relationship** between business categories, average popularity or average ratings, and the availability of Free Bikes or Total Bike Slots.
- This held true both **before and after bias correction**, suggesting a weak influence of surrounding locations on bike availability.

- Cleaned Dataframe Scatterplot Visualisations
    ![Scatterplot of Free Bikes Vs Ratings](https://github.com/Mitch-P-Analyst/CityBike-Lisbon-Analysis/blob/main/images/Free_Bkes_Ratings_Vis.png?raw=true)
    - *Scatterplot of Free Vikes Vs Ratings*
    
    ![Scatterplot of Free Bikes Vs Categories](https://github.com/Mitch-P-Analyst/CityBike-Lisbon-Analysis/blob/main/images/Free_Bikes_Cates_Vis.png?raw=true)
    - *Scatterplot of Free Bikes Vs Categories*

    ![Pairplot of Bike Stations](https://github.com/Mitch-P-Analyst/CityBike-Lisbon-Analysis/blob/main/images/Pairplot_Vis.png?raw=true)
    - *Pairplot of all Bike Stations*

### OLS Regression Model Findings
- Pearson R correlation and VIF analyses confirmed **low multicollinearity for business categories**, with a proposed threshold of 5. However, little predictive strength in further analysis.
    - VIF analysis did find **high multicollinearity among ratings and popularity**. These variables removed from Multivariate Regression Analysis. 
- A linear regression model was applied using **Average Popularity of Locations** as the independent variable, and Fr**ee Bikes and Total Bike Slots** as the dependent variables. 
    - Weak linear relationships were observed between top independent variables and dependent variables.
   
- Selecting Top Independeent Variables:
    - Coefficients
        - Each additional bar within 298 metres is associated with **1.67 more** free bikes.
        - Each additional café is associated with **0.74 fewer** free bikes.
    - p-value
        - Both Correlations Statistically Significant at the level of 5% alpha
    - Adjusted R-squared of multivariate models increased:
        - The model explains approximately **17.9% of the variance** in free bike availability, indicating low predictive power.


## Conclusion
- The Foursquare API provided geospatial location context, but within Lisbon city, the nearby venue data utilised does not show strong predictive influence on city bike station metrics.
- No strong predictive relationships or correlations were observed between **Location Categories** or **Location Ratings** with **Total Bike Slots** or **Free Bikes** of CityBike Stations.
    - Location Categories are a **poor** predictor of both Bike dependent variables
    - Location Ratings & Popularity are a **poor** predictor of both Bike dependent variables
- However, **bar_count** and **cafe_counts** offered the strongest available predictive value for **Free Bikes** accounting for **approximately 17.9% of the variance**.

- Alternative predictor variables and features not present in this analysis may be more valuable in predicting bike availability than nearby venue types or ratings

### Future Goals
- Additional Independent Variables
    - Examples
        - Time Of Day
        - Season
    - CityBike API Data Timestamp = 12:39 AM Sunday, July 27 2025
        - Further Goal to observe trends across daily, weekly & monthly timestamps
- Cleaning further of categories
    - Top 4 cleaned categories utilised from CityBike API
        - Restaurant
        - Bar
        - Cafe
        - Coffee
    - Further cleaning and grouping of broader categories
        - Examples 
            - Public / Government
            - Retail
            - Hospitality

- Additional Statistical Analysis
    - Statistical Testing 
        - Models
            - Plot residuals vs fitted values
            - Generate Q-Q plot of residuals
            - Add a line of best fit to scatterplots using sns.regplot()
        - T-Tests to identify acquired sample data of locations as a reflection of true populations.
            - As a result of restricted FOURSQUARE API Response limits.
        - However, given poor predictor variables, significance of successful Statistical Testing minimal due to infered results
       


### Challenges 
- Selection Bias in FOURSQUARE API Reponses.
    - Restricted responses from FOURSQUARE API required a limitation of 50 locations for Bike Station Coordiantes.
    - Restricted data available to assess full population of locations surrounding Bike Station, requiring cleaning and reduction of location distance threshold.

- Low Predictive Strength of Independent Variables.
    - Low/Weak linear relationships from all geospatially available data with our CityBike Stations dependent variables.
    - Resulting conclusions are low model effectiveness and future goals to identify alternative independent variables.

- Multicollinearity
    - Strong multicollinearity present among top linear correlated variables, Average Rating and Average Popularity. 
    - Separation of multicollinearity variables was required to meet assumptions for accurate model predictions.



