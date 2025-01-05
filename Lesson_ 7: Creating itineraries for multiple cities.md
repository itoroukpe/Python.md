In this lesson, you will use everything you have seen so far to plan the perfect vacation around the world!

To get started, import some helper functions:

from helper_functions import print_llm_response, get_llm_response, display_table
from IPython.display import Markdown
import csv
Reading travel itineraries from a CSV file
First, define a new function that reads data stored in a CSV file and returns it as a dictionary variable:

def read_csv(file):
    f = open(file, "r")
    
    csv_reader = csv.DictReader(f)
    data = []
    for row in csv_reader:
        data.append(row)
    f.close()
    
    return data
Next, load itineraries from itinerary.csv using the function you just defined (notice how much less code this is!) and then display the table of itineraries:

# Read the itinerary.csv file
itinerary = read_csv("itinerary.csv")
​
# Display the itinerary
display_table(itinerary)
Reading restaurant information from food journal entries
Now create a new function called read_journal that reads in the contents of a plain text file with '.txt' extension and stores it into a string variable:

# The function called 'read_journal'
def read_journal(journal_file):
    f = open(journal_file, "r")
    journal = f.read() 
    f.close()
​
    # Return the journal content
    return journal
Note that you used this function in an earlier lesson - now you know how it works!

You can now use the read_journal function to read in a food journal file - let's start with Sydney:

journal = read_journal("sydney.txt")
​
print(journal)
Write a prompt that extracts restaurant and specialty dish information from the journal text and stores it in CSV format:

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
...
​
Journal entry:
{journal}
"""
​
# Print the prompt
print_llm_response(prompt)
Read in restaurant information from Sydney.csv file that was created for you and display it using the display_table function:

# Use the read_csv function
sydney_restaurants = read_csv("Sydney.csv")
​
display_table(sydney_restaurants)
Creating detailed itineraries with restaurant suggestions
In this section, you'll combine the data in the journal and the itinerary to create a detailed plan for your visit to Sydney.

To access Sydney's data in the itinerary list, you have to use index '6' since Sydney is the seventh trip destination.

# Select Sydney from the 'itinerary' list
trip_stop = itinerary[6]
Next, store all the information from that trip_stop, as well as the restaurant information you read in above, in separate variables:

city = trip_stop["City"]
country = trip_stop["Country"]
arrival = trip_stop["Arrival"]
departure = trip_stop["Departure"]
restaurants = sydney_restaurants
Pass all of this information in a detailed prompt to an LLM to create a detailed itinerary:

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
​
# Print the LLM response in Markdown format
display(Markdown(response))
Create detailed itineraries for all the cities in your trip
You'll use a 'for' loop to iterate over all the cities in the itinerary list and create a detailed itinerary for each location:

# Create an empty dictionary to store the itinerary for each destination
detailed_itinerary = {}
​
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
You can now access the detailed itinerary for any city by passing in the city name as the key to the detailed_itinerary dictionary:

# Print in Markdown format
display(Markdown(detailed_itinerary["Tokyo"]))
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
# Update the next line of code to view a different city
display(Markdown(detailed_itinerary["YOUR CITY HERE"]))
Congratulations on completing this course! 🎉🎉🎉
Please go onto the fourth and final course of this sequence where you'll learn how to extend the capabilities of Python using code written by other programmers!

