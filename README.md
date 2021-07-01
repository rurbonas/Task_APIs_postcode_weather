## Using APIs
### Input Postcode and get weather prediction of the postcode's longitude and latitude location

```python
import requests

postcode = input("Please write your postcode: ").lower().replace(' ', '')
check_response_postcode = requests.get("https://api.postcodes.io/postcodes/{}".format(postcode))

if check_response_postcode.status_code:
    print(f"The postcode is valid and status code is {check_response_postcode.status_code}")
    #print(f"The weather there is: {check_response_weather}")
else:
    print("Oops something went wrong")

response_dict = check_response_postcode.json()
# Create a dictionary from the scraped JSON data
result_dict = response_dict['result']
# print(result_dict)
longitude = 0
latitude = 0
# Get the longitude and latitude of the postcode entered
for key in result_dict.keys():
    if key == 'longitude':
        longitude = result_dict[key]
    elif key == 'latitude':
        latitude = result_dict[key]

    #print(f"The name of the key is {key} and the value inside it is {result_dict[key]}")
print(f"Your longitude {longitude} and latitude {latitude}")

# Use longitude and latitude of the postcode to find precise weather from a different API service
weather = requests.get("https://api.openweathermap.org/data/2.5/weather?lat={lat}&lon={lon}&appid=b0bf086f31215eaeb81487d50e11f5bd".format(lat=latitude, lon=longitude))
weather_dict = weather.json()
print(f"This location has: {weather_dict['weather'][0]['description']}")

```