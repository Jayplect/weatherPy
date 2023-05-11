## Desciption
Data's true power is its ability to definitively answer questions. In this project, I explored Python requests, APIs, and JSON traversals to answer a fundamental question: "What is the weather like as we approach the equator?" The answer may seem pretty obvious but this project seeks to prove that.

## Dependencies used
<img width="120" src = https://user-images.githubusercontent.com/107348074/236379825-80dc02bc-46c1-46fa-9634-dc28cdcb5704.png>
<img width="120" src = https://user-images.githubusercontent.com/107348074/236379877-e0e3b90e-217f-4700-ade2-df8b5ef8f23b.png>
<img width="120" src = https://user-images.githubusercontent.com/107348074/236379504-0f0e8534-6435-4924-b72d-283946e03c4b.png>
<img width="120" src =https://user-images.githubusercontent.com/107348074/236379730-0286f397-c9e0-4e0c-a91c-e07d64f6ceec.png>


## Summary of Dataset
I created a set of latitudes and longitudes using the random function from numpy (Example 1) and the used these cordinates combinations to identify the nearest cities. Citipy was used to access the cities based on the coordinates (Example 2). Note that the city data generated is based on random coordinates and would thus differ for each query.

  Example 1: create a set of random lat and lng combinations

      lats = np.random.uniform(lat_range[0], lat_range[1], size=1500)
      lngs = np.random.uniform(lng_range[0], lng_range[1], size=1500)
      lat_lngs = zip(lats, lngs)
      
  Example 2: Identify nearest city for each lat, lng combination
  
      for lat_lng in lat_lngs:
          city = citipy.nearest_city(lat_lng[0], lat_lng[1]).city_name
          if city not in cities:
            cities.append(city)
          
## Project Steps
### Step 1: Merging both data sets 

### Step 2: Summary Statistics 

### Step 2: Visualizations
-
## Summary of Results 

## References
Data for this dataset was generated by edX Boot Camps LLC, and is intended for educational purposes only.

