# Accuweather-Hartford
Creating a Excel worksheet to capiture and track daily high and low temps by day of month.
import requests
from bs4 import BeautifulSoup
from openpyxl import Workbook
import certifi

# URL of the weather page
url = "https://www.accuweather.com/en/us/windsor-locks/06096/daily-weather-forecast/337525?page=0"

# Send a GET request to the URL with certifi for SSL verification
response = requests.get(url, verify=certifi.where())

# Parse the HTML content
soup = BeautifulSoup(response.content, "html.parser")

# Find the section containing the daily temperatures
daily_temps = soup.find_all("div", class_="monthly-daypanel")

# List to store the data
weather_data = []

# Extract and store the high and low temperatures by day
for day in daily_temps:
    date = day.find("span", class_="date").text.strip()
    high_temp = day.find("span", class_="high").text.strip()
    low_temp = day.find("span", class_="low").text.strip()
    weather_data.append([date, high_temp, low_temp])
    print(f"Date: {date}, High: {high_temp}, Low: {low_temp}")  # Debugging print statement

# Create a new Excel workbook and sheet
wb = Workbook()
ws = wb.active
ws.title = "Weather Data"

# Add headers
ws.append(["Date", "High Temperature", "Low Temperature"])

# Add data to the sheet
for data in weather_data:
    ws.append(data)

# Save the workbook
wb.save("weather_data.xlsx")

print("Data saved to weather_data.xlsx")
