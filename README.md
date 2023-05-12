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
The first step was to make a call for weather information (e.g., 
# Set the API base URL
    # Save config information.
url = "http://api.openweathermap.org/data/2.5/weather?"
units = "metric"

    # Build partial query URL
query_url= f"{url}appid={weather_api_key}&units={units}&q="

# Define an empty list to fetch the weather data for each city
city_data = []

# Print to logger
print("Beginning Data Retrieval     ")
print("-----------------------------")

# Create counters
record_count = 1
set_count = 1

# Loop through all the cities in our list to fetch weather data
for i, city in enumerate(cities):
        
    # Group cities in sets of 50 for logging purposes
    if (i % 50 == 0 and i >= 50):
        set_count += 1
        record_count = 0

    # Create endpoint URL with each city
    city_url = query_url + city
    
    # Log the url, record, and set numbers
    print("Processing Record %s of Set %s | %s" % (record_count, set_count, city))

    # Add 1 to the record count
    record_count += 1

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

        # Append the City information into city_data list
        city_data.append({"City": city, 
                          "Lat": city_lat, 
                          "Lng": city_lng, 
                          "Max Temp": city_max_temp,
                          "Humidity": city_humidity,
                          "Cloudiness": city_clouds,
                          "Wind Speed": city_wind,
                          "Country": city_country,
                          "Date": city_date})

    # If an error is experienced, skip the city
    except:
        print(f"Record for {city} not found. Skipping...")
        pass

### Step 2: Visualizations

### Step 3: Staistics

### Step 4: Querying and Mapping
Lastly, I filtered for my ideal city using some specific criteria (Example xx), queried the first hotel located wihtin 10km of coordinates (Example xx) and rendered their locations on a map plot (Example xx). The criteria used for selecting my ideal city included cities with max temperatures lower than 27 degrees but higher than 21, cites with wind speed less than 4.5 m/s and cities with clear skies (i.e., zero cloudiness). In order to ensure that no key or value error arise due to unavailable data, I used the try/except method to by pass missing data (Example xx).
  
  #Example xx: Narrow down cities that fit criteria and drop any results with null values 

        df = df.loc[(df["Max Temp"] < 27) & (df["Max Temp"] > 21) & (df["Wind Speed"] < 4.5) & (df["Cloudiness"] == 0)]
        #Drop any rows with null values using the dropna() function
        df = df.dropna()

  #Example xx: Set parameters to query hotel nearest to the city
        
        radius =10000
        params = {"categories": "accommodation.hotel", "apiKey": geoapify_key}
        # Set base URL
        base_url = "https://api.geoapify.com/v2/places"
        # Make and API request using the params dictionary
        response = requests.get(base_url, params)
        # Convert the API response to JSON format
        data = response.json()

   #Example xx: Try/Except method to by pass missing data- Grab the first hotel from the results and store the name in the hotel_df DataFrame
      
      try:
          df.loc[index, "col"] = name_address["features"][0]["properties"]["name"]
      except (KeyError, IndexError):
          # If no hotel is found, set the hotel name as "No hotel found".
          df.loc[index, "col"] = "No hotel found"

All Cities used in visualizations:

<img width="1000" src =https://github.com/Jayplect/python-api-challenge/assets/107348074/14b461d6-caf2-444f-b1ba-9d0e7e108c06>
 
 Example xx: My Ideal Cities
 
<img width="1000" src =https://github.com/Jayplect/python-api-challenge/assets/107348074/e8a9d71d-9427-42b8-99ac-96d19e088a19>


## Summary of Results 

## References
Data for this dataset was generated by edX Boot Camps LLC, and is intended for educational purposes only.

