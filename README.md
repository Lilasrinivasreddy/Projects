import pandas as pd

# Define the hardcoded latitude and longitude values
state_coordinates = {
    'CA': {'Latitude': 36.7783, 'Longitude': -119.4179},
    'FL': {'Latitude': 27.7663, 'Longitude': -81.6868},
    'GA': {'Latitude': 32.1656, 'Longitude': -82.9001},
    'IL': {'Latitude': 39.7817, 'Longitude': -89.6501},
    'NJ': {'Latitude': 40.0583, 'Longitude': -74.4057},
    'NY': {'Latitude': 43.2994, 'Longitude': -74.2179},
    'OH': {'Latitude': 40.4173, 'Longitude': -82.9071},
    'PA': {'Latitude': 41.2033, 'Longitude': -77.1945},
    'TX': {'Latitude': 31.9686, 'Longitude': -99.9018}
}

# Load your DataFrame with the 'State' column
df = pd.read_csv('your_data.csv')  # Replace 'your_data.csv' with your actual data file

# Add latitude and longitude columns based on the 'State' column
df[['Latitude', 'Longitude']] = df['State'].map(state_coordinates)

# Print the resulting DataFrame
print(df)
