import pandas as pd
from geopy.geocoders import Nominatim

# Define the list of states
states = ['ca', 'fl', 'ga', 'IL', 'Miami', 'NJ', 'ny', 'Ohio', 'Pennsylvania', 'texas']

# Create an empty DataFrame
df = pd.DataFrame({'State': states})

# Create geocoder object
geolocator = Nominatim(user_agent='my_geocoder')

# Function to get latitude and longitude
def get_lat_long(location):
    try:
        geocode = geolocator.geocode(location)
        return geocode.latitude, geocode.longitude
    except:
        return None, None

# Apply the function to the 'State' column
df['Latitude'], df['Longitude'] = zip(*df['State'].map(get_lat_long))

# Print the resulting DataFrame
print(df)
