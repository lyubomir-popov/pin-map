import csv
import time
import requests

INPUT_FILE = "data.csv"
OUTPUT_FILE = "data-geocoded.csv"

def geocode(query):
    url = "https://nominatim.openstreetmap.org/search"
    params = {
        "q": query,
        "format": "json",
        "limit": 1
    }
    headers = {
        "User-Agent": "YourApp/1.0 (you@example.com)"
    }
    response = requests.get(url, params=params, headers=headers)
    data = response.json()
    if data:
        return data[0]["lat"], data[0]["lon"]
    else:
        return "", ""

with open(INPUT_FILE, newline='', encoding='utf-8') as csvfile:
    reader = csv.DictReader(csvfile)
    rows = []

    for row in reader:
        query = f"{row['city']}, {row['country']}"
        print(f"Geocoding: {query}")
        lat, lon = geocode(query)
        row["lat"] = lat
        row["lon"] = lon
        rows.append(row)
        time.sleep(1)  # Respect Nominatim's rate limit

fieldnames = list(rows[0].keys())
with open(OUTPUT_FILE, "w", newline='', encoding='utf-8') as csvfile:
    writer = csv.DictWriter(csvfile, fieldnames=fieldnames)
    writer.writeheader()
    writer.writerows(rows)

print(f"Done! Saved to {OUTPUT_FILE}")
