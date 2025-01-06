## Lesson 7: Creating itineraries for multiple cities
In this lesson, you will use everything you have seen so far to plan the perfect vacation around the world!

To get started, import some helper functions:
```bash
from helper_functions import print_llm_response, get_llm_response, display_table
from IPython.display import Markdown
import csv
```
Reading travel itineraries from a CSV file
First, define a new function that reads data stored in a CSV file and returns it as a dictionary variable:
```bash
def read_csv(file):
    f = open(file, "r")
    
    csv_reader = csv.DictReader(f)
    data = []
    for row in csv_reader:
        data.append(row)
    f.close()
    
    return data
```
Next, load itineraries from itinerary.csv using the function you just defined (notice how much less code this is!) and then display the table of itineraries:
```bash
# Read the itinerary.csv file
itinerary = read_csv("itinerary.csv")
​```
```bash
# Display the itinerary
display_table(itinerary)
```

```bash
Arrival	Departure	City	Country
July-01	July-08	New York	USA
July-09	July-16	Rio de Janeiro	Brazil
July-17	July-24	Cape Town	South Africa
July-25	August-01	Istanbul	Turkey
August-02	August-09	Paris	France
August-10	August-17	Tokyo	Japan
August-18	August-25	Sydney	Australia
```
Reading restaurant information from food journal entries
Now create a new function called read_journal that reads in the contents of a plain text file with '.txt' extension and stores it into a string variable:
```bash
# The function called 'read_journal'
def read_journal(journal_file):
    f = open(journal_file, "r")
    journal = f.read() 
    f.close()
​
    # Return the journal content
    return journal
```
Note that you used this function in an earlier lesson - now you know how it works!

You can now use the read_journal function to read in a food journal file - let's start with Sydney:
```bash
journal = read_journal("sydney.txt")
​
print(journal)
```
My culinary adventure began at Saint Peter, a renowned seafood restaurant in Paddington. This place is a temple to Australian seafood, and the "Murray Cod" was a revelation. The fish, sourced from the Murray River, was perfectly cooked, with a crispy skin and tender, flaky flesh. It was served with a simple yet flavorful accompaniment of seasonal vegetables, allowing the quality of the fish to shine. The restaurant's dedication to sustainability and nose-to-tail seafood cooking added an educational aspect to the delicious meal.

Next, I visited Billy Kwong in Potts Point, where celebrated chef Kylie Kwong puts a unique spin on modern Australian cuisine using native ingredients. The standout dish here was the "Crispy Skin Duck with Davidson’s Plum Sauce." The duck was cooked to perfection, with a rich, flavorful meat and delightfully crispy skin, complemented by the tart and slightly sweet Davidson’s plum sauce. This dish was a perfect example of how traditional recipes can be elevated with local, indigenous ingredients, creating something both familiar and new.

In search of a true Australian pub experience, I headed to The Lord Nelson Brewery Hotel in The Rocks. This historic pub serves up hearty, classic Australian fare, and the "Roast Lamb" was exactly what I was craving. The lamb, roasted to tender perfection, was served with a medley of root vegetables and a rich gravy, making for a comforting and satisfying meal. Paired with one of their house-brewed ales, it was a quintessential Aussie pub experience that I would highly recommend.

I couldn't miss out on trying some of the famous Australian barbecue, so I headed to Vic's Meat Market at the Sydney Fish Market. The "BBQ Beef Brisket" was a highlight, slow-cooked to achieve a melt-in-the-mouth texture, and served with a tangy barbecue sauce. The smoky, rich flavor of the brisket was enhanced by the vibrant, outdoor setting of the market, where the aroma of grilling meat filled the air.

To round off my exploration of local cuisine, I visited Bennelong, located within the iconic Sydney Opera House. This fine dining restaurant celebrates Australian produce in every dish. The "Sydney Rock Oysters" were an exquisite start to the meal, served with a delicate vinaigrette that highlighted their briny freshness. The oysters, sourced from local waters, were plump and succulent, offering a pure taste of the sea.


Write a prompt that extracts restaurant and specialty dish information from the journal text and stores it in CSV format:
```bash
# Write the prompt
prompt = f"""Please extract a comprehensive list of the restaurants 
and their respective specialties mentioned in the following journal entry. 
Ensure that each restaurant name is accurately identified and listed. 
Provide your answer in CSV format, ready to save. 
Exclude the "```csv" declaration, don't add spaces after the comma, include column headers.
​
Format:
Restaurant, Specialty
Res_1, Sp_1
​
Journal entry:
{journal}
"""
​
# Print the prompt
print_llm_response(prompt)
```
Restaurant,Specialty
Saint Peter,Murray Cod
Billy Kwong,Crispy Skin Duck with Davidson’s Plum Sauce
The Lord Nelson Brewery Hotel,Roast Lamb
Vic's Meat Market,BBQ Beef Brisket
Bennelong,Sydney Rock Oysters
Read in restaurant information from Sydney.csv file that was created for you and display it using the display_table function:
```bash
# Use the read_csv function
sydney_restaurants = read_csv("Sydney.csv")
```​

```bash
display_table(sydney_restaurants)
```
Restaurant	Specialty
Saint Peter	Murray Cod
Billy Kwong	Crispy Skin Duck with Davidson’s Plum Sauce
The Lord Nelson Brewery Hotel	Roast Lamb
Carriageworks Farmers Market	Kangaroo Pie
Vic's Meat Market	BBQ Beef Brisket
Bennelong	Sydney Rock Oysters

Creating detailed itineraries with restaurant suggestions
In this section, you'll combine the data in the journal and the itinerary to create a detailed plan for your visit to Sydney.

To access Sydney's data in the itinerary list, you have to use index '6' since Sydney is the seventh trip destination.
```bash
# Select Sydney from the 'itinerary' list
trip_stop = itinerary[6]
```
Next, store all the information from that trip_stop, as well as the restaurant information you read in above, in separate variables:
```bash
city = trip_stop["City"]
country = trip_stop["Country"]
arrival = trip_stop["Arrival"]
departure = trip_stop["Departure"]
restaurants = sydney_restaurants
```
Pass all of this information in a detailed prompt to an LLM to create a detailed itinerary:
```bash
# Write the prompt
prompt = f"""I will visit {city}, {country} from {arrival} to {departure}. 
Create a daily itinerary with detailed activities. 
Designate times for breakfast, lunch, and dinner. 
​
I want to visit the restaurants listed in the restaurant dictionary 
without repeating any place. Make sure to mention the specialty
that I should try at each of them.
​
Restaurant dictionary:
{restaurants}
​
"""
​
response = get_llm_response(prompt)
```

```bash
# Print the LLM response in Markdown format
display(Markdown(response))
```
Sydney Itinerary: August 18 - August 25
Day 1: August 18 (Saturday)
Breakfast: 8:00 AM - Café in your hotel or nearby café
Morning Activity: Explore Circular Quay and the Sydney Opera House
Lunch: 12:30 PM - Bennelong
Specialty: Sydney Rock Oysters
Afternoon Activity: Visit the Royal Botanic Garden
Dinner: 7:00 PM - Restaurant in the area or hotel dining
Day 2: August 19 (Sunday)
Breakfast: 8:00 AM - Local café
Morning Activity: Visit The Rocks and explore the weekend markets
Lunch: 12:30 PM - The Lord Nelson Brewery Hotel
Specialty: Roast Lamb
Afternoon Activity: Walk across the Sydney Harbour Bridge
Dinner: 7:00 PM - Restaurant in The Rocks area
Day 3: August 20 (Monday)
Breakfast: 8:00 AM - Local café
Morning Activity: Visit Taronga Zoo
Lunch: 12:30 PM - Carriageworks Farmers Market
Specialty: Kangaroo Pie
Afternoon Activity: Explore the nearby suburb of Newtown
Dinner: 7:00 PM - Restaurant in Newtown
Day 4: August 21 (Tuesday)
Breakfast: 8:00 AM - Local café
Morning Activity: Visit Bondi Beach and take the coastal walk to Coogee
Lunch: 12:30 PM - Café at Coogee Beach
Afternoon Activity: Relax at Coogee Beach
Dinner: 7:00 PM - Restaurant in Coogee
Day 5: August 22 (Wednesday)
Breakfast: 8:00 AM - Local café
Morning Activity: Visit the Art Gallery of New South Wales
Lunch: 12:30 PM - Billy Kwong
Specialty: Crispy Skin Duck with Davidson’s Plum Sauce
Afternoon Activity: Explore Darling Harbour
Dinner: 7:00 PM - Restaurant in Darling Harbour
Day 6: August 23 (Thursday)
Breakfast: 8:00 AM - Local café
Morning Activity: Visit the Sydney Tower Eye for panoramic views
Lunch: 12:30 PM - Vic's Meat Market
Specialty: BBQ Beef Brisket
Afternoon Activity: Explore the Queen Victoria Building
Dinner: 7:00 PM - Restaurant in the CBD
Day 7: August 24 (Friday)
Breakfast: 8:00 AM - Local café
Morning Activity: Day trip to the Blue Mountains (visit Scenic World)
Lunch: 12:30 PM - Café in the Blue Mountains
Afternoon Activity: Explore hiking trails and scenic views
Dinner: 7:00 PM - Restaurant in Katoomba or return to Sydney for dinner
Day 8: August 25 (Saturday)
Breakfast: 8:00 AM - Local café
Morning Activity: Last-minute shopping or visit a local market
Lunch: 12:30 PM - Café in the area
Afternoon Activity: Relax at a park or beach
Dinner: 7:00 PM - Final dinner at a restaurant of your choice
Enjoy your trip to Sydney!

Create detailed itineraries for all the cities in your trip
You'll use a 'for' loop to iterate over all the cities in the itinerary list and create a detailed itinerary for each location:
```bash
# Create an empty dictionary to store the itinerary for each destination
detailed_itinerary = {}
​```

```bash
 # Use the 'for' loop over the 'itinerary' list   
for trip_stop in itinerary:
    city = trip_stop["City"]
    country = trip_stop["Country"]
    arrival = trip_stop["Arrival"]
    departure = trip_stop["Departure"]
​
    rest_dict = read_csv(f"{city}.csv")
    
    print(f"Creating detailed itinerary for {city}, {country}.")
    
    prompt = f"""I will visit {city}, {country} from {arrival} to {departure}. 
    Create a daily itinerary with detailed activities. 
    Designate times for breakfast, lunch, and dinner. 
​
    I want to visit the restaurants listed in the restaurant dictionary without repeating any place.
    Make sure to mention the specialty that I should try at each of them.
​
    Restaurant dictionary:
    {rest_dict}
​
    """
    # Store the detailed itinerary for the city to the dictionary
    detailed_itinerary[city] = get_llm_response(prompt)
```
You can now access the detailed itinerary for any city by passing in the city name as the key to the detailed_itinerary dictionary:

```bash
# Print in Markdown format
display(Markdown(detailed_itinerary["Tokyo"]))
```
Try it yourself!
Update the code below to check out the itinerary for another city.

Options:

Cape Town
Istanbul
New York
Paris
Rio de Janeiro
Sydney
Tokyo
```bash
# Update the next line of code to view a different city
display(Markdown(detailed_itinerary["Cape Town"]))
```
Congratulations on completing this course! 🎉🎉🎉
Please go onto the fourth and final course of this sequence where you'll learn how to extend the capabilities of Python using code written by other programmers!
