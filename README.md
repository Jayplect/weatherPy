## Desciption
Data's true power is its ability to definitively answer questions. In this project, I explored Python requests, APIs, and JSON traversals to answer a fundamental question: "What is the weather like as we approach the equator?" The answer may seem pretty obvious but this project seeks to prove that.

## Dependencies used
<img width="120" src = https://user-images.githubusercontent.com/107348074/236379825-80dc02bc-46c1-46fa-9634-dc28cdcb5704.png>
<img width="120" src = https://user-images.githubusercontent.com/107348074/236379877-e0e3b90e-217f-4700-ade2-df8b5ef8f23b.png>
<img width="120" src = https://user-images.githubusercontent.com/107348074/236379504-0f0e8534-6435-4924-b72d-283946e03c4b.png>
<img width="120" src =https://user-images.githubusercontent.com/107348074/236379730-0286f397-c9e0-4e0c-a91c-e07d64f6ceec.png>
<img width="120" src =https://github.com/Jayplect/python-api-challenge/assets/107348074/5462064f-17bc-4b63-97f0-034eb771cc00>

## Summary of Dataset
I created a set of latitudes and longitudes using the random function from numpy (Example 1) and the used these cordinates combinations to identify the nearest cities. Citipy was used to access the nearest cities based on the coordinates (Example 2). Note that the city data generated is based on random coordinates and would thus differ for each query.

  #Example 1: create a set of random lat and lng combinations
      
      lats = np.random.uniform(lat_range[0], lat_range[1], size=1500)
      lngs = np.random.uniform(lng_range[0], lng_range[1], size=1500)
      lat_lngs = zip(lats, lngs)
      
  #Example 2: Identify nearest city for each lat, lng combination
      
      for lat_lng in lat_lngs:
          city = citipy.nearest_city(lat_lng[0], lat_lng[1]).city_name
          if city not in cities:
            cities.append(city)
          
## Project Steps
### Step 1: Making API calls for Weather Data 
The first step was to make a call, using the OpenWeatherMap API (Example 3), for latitude and longitude coordinates as well as weather information (e.g., max temp, humidity, cloudiness, wind speed, country, and date for each of the cities generated above. The try/except method was used to by-pass missing data (Example 4).

#Example 3: Set the API base URL

    # Save config information.
        url = "http://api.openweathermap.org/data/2.5/weather?"
        units = "metric"
    # Build partial query URL
        query_url= f"{url}appid={weather_api_key}&units={units}&q=
    # Loop through all the cities in our list to fetch weather data
        for i, city in enumerate(cities):
            # Create endpoint URL with each city
            city_url = query_url + city
            
  #Example 4: Try/except method was used to by-pass missing data for the coordinates and weather informations
   
      # Run an API request for each of the cities
          try:
            # Parse the JSON and retrieve data
            city_weather = requests.get(city_url).json()
            # Parse out latitude, longitude, max temp, humidity, cloudiness, wind speed, country, and date
            city_lat = city_weather["coord"]["lat"]
            city_lng = city_weather["coord"]["lon"]
            city_max_temp = city_weather["main"]["temp_max"]
            city_humidity = city_weather["main"]["humidity"]
            city_clouds = city_weather["clouds"]["all"]
            city_wind = city_weather["wind"]["speed"]
            city_country = city_weather["sys"]["country"]
            city_date = city_weather["dt"]
      # If an error is experienced, skip the city
          except:
            pass

### Step 2: Visualizations
After using the OpenWeatherMap API to retrieve weather data from the cities list generated in step above,I created a series of scatter plots to showcase the following relationships: Latitude vs. Temperature (Plot 1), Latitude vs. Humidity, Latitude vs. Cloudiness, Latitude vs. Wind Speed. 

Plot 1: Chart showing  relationship between Latitude and Temperature

<img width="1000" src =https://github.com/Jayplect/python-api-challenge/assets/107348074/64ee1cd3-7666-44ff-9cde-74db44f9f27b>

### Step 3: Staistics
In this step, I computed the linear regression for each relationship created above by separating the plots into Northern Hemisphere (greater than or equal to 0 degrees latitude) and Southern Hemisphere (less than 0 degrees latitude). To save time, I defined a function (Example 5) in order to create the linear regression plots.

#Example 5: Function to create Linear Regression plots

        def plot_linear_regresssion (x_values, y_values, title):
            #regression line
            (slope, intercept, rvalue, pvalue, stderr) = linregress(x_values, y_values)
            y_regress = slope * x_values + intercept
            line_eqn = f"y={round(slope,2)}x+{round(intercept,2)}"
            #plot
            plt.figure(figsize = (9,7))
            plt.scatter(x_values, y_values, s = 60)
            plt.plot(x_values, y_regress, "r-")
            #text coordinates
            text_y_coord = max(y_values)/10
            if min(x_values) < 0:
                text_x_coord = min(x_values)
            elif max(x_values) > 0:
                text_x_coord = max(x_values)/10
            #line equation
            plt.annotate(line_eqn,(text_x_coord,text_y_coord), fontsize=12,color="red", size= 16)
            print(f"The r-squared value is {rvalue**2}")
            #label and graph properties
            plt.ylabel(title.split("vs")[0], size = 14 )
            plt.xlabel(title.split("vs")[1], size = 14)
            plt.yticks(size = 14)
            plt.xticks(size = 14)
            plt.show()
            
Plot 2: An example of Scatter Plot showing linear regression between Max Temperature and Latitude in the Northern Hemisphere (i.e., latitude >= 0)

<img width="1000" src =https://github.com/Jayplect/python-api-challenge/assets/107348074/f270d29a-7492-431f-8a53-1bc79731556a>

Plot 3: Scatter Plot showing linear regression between Max Temperature and Latitude in the Southern Hemisphere (i.e., latitude < 0)

<img width="1000" src =https://github.com/Jayplect/python-api-challenge/assets/107348074/4f4f24e2-0bc9-4412-ba21-e5b97ffd9ef4>

### Step 4: Querying and Mapping
Lastly, I filtered for my ideal city using some specific criteria (Example 6), queried the first hotel located wihtin 10km of coordinates (Example 7) and rendered their locations on a map plot (Plot 4). The criteria used for selecting my ideal city included cities with max temperatures lower than 27 degrees but higher than 21, cites with wind speed less than 4.5 m/s and cities with clear skies (i.e., zero cloudiness). In order to ensure that no key or value error arise due to unavailable data, I used the try/except method to by pass missing data (Example 8).
  
  #Example 6: Narrow down cities that fit criteria and drop any results with null values 

        df = df.loc[(df["Max Temp"] < 27) & (df["Max Temp"] > 21) & (df["Wind Speed"] < 4.5) & (df["Cloudiness"] == 0)]
        #Drop any rows with null values using the dropna() function
        df = df.dropna()

  #Example 7: Set parameters to query hotel nearest to the city
        
        radius =10000
        params = {"categories": "accommodation.hotel", "apiKey": geoapify_key}
        # Set base URL
        base_url = "https://api.geoapify.com/v2/places"
        # Make and API request using the params dictionary
        response = requests.get(base_url, params)
        # Convert the API response to JSON format
        data = response.json()

   #Example 8: Try/Except method to by pass missing data- Grab the first hotel from the results and store the name in the hotel_df DataFrame
      
      try:
          df.loc[index, "col"] = name_address["features"][0]["properties"]["name"]
      except (KeyError, IndexError):
          # If no hotel is found, set the hotel name as "No hotel found".
          df.loc[index, "col"] = "No hotel found"

Plot 4: Map showing all Cities generated using CityPy

<img width="1000" src =https://github.com/Jayplect/python-api-challenge/assets/107348074/14b461d6-caf2-444f-b1ba-9d0e7e108c06>
 
 Plot 5: My Ideal Cities based on the criteria for cities (> 21 ${^oC}$ max_temperatures < 27 ${^oC}$, wind_speed < 4.5 m/s, cloudiness = zero 
 
<img width="1000" src =https://github.com/Jayplect/python-api-challenge/assets/107348074/e8a9d71d-9427-42b8-99ac-96d19e088a19>

## Summary of Results
- From Plot 1, temperature becomes less warmer as we approach higher latitudes in the Northern hemisphere (latitudes >= 0) and vice-versa. On the other hand, temperatures feels warmer as we near the equator and vice-versa in the southern hemisphere (latitudes < 0).
- As observed in Plot 2, aprroximately 70% of the variance in the response varaible- Max temperature, can be explained by the independent variable- Latitude.  Whereas, in plot 3, only about 60% of the variability in the outcome data can be explained by the model.

## References
Data for this dataset was generated by edX Boot Camps LLC, and is intended for educational purposes only.

