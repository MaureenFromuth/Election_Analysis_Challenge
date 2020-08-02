# Election Anaylsis Challenge

## Overview of Project

### Purpose  

Our primary customers for this project, Tom and Seth, work as employees for the Colorado Board of Elections.  They have asked for support in conducting an election audit of the [tabular results](https://github.com/MaureenFromuth/Election_Analysis_Challenge/blob/master/election_results.csv) for a US Congressional Precinct in Colorado.  This data is a compilation of three primary sources to include mail-in ballots, punch cards, and direct reporting electronic counting machines.  The data is a record of each vote (row) and includes the following fields:
- Ballot ID for the voter
- County the voter voted in
- Candidate the voter voted for

Tom and Seth are conducting analysis of the election data to determine a numbner of different analytic conclusions about the results of the election.  These conclusions include:
- The total number of votes in the election
- The list of candidates who received votes
- How many votes each candidate received
- The percentage of votes each candidate recieved out of the overall vote count
- The winner of the election

After concluding the winner of the election, Tom and Seth also asked for support in anayzing the overall participation per county within the election.  Using the election data already provided, they want to identify the following conclusions about election participation:
- How many votes were conducted in each county
- What was the percentage of the overall vote occurred in each county
- Which county had the largest voter participation

Ultimately, Tom and Seth are seeking to build out code that can not only automate this election, but can be used as baseline code to calculate other elections across the state of Colorado.

## Results
To conduct analysis we used Jupyter Notebook as an IDE and Python 3.7.6 for a code baseline.  We also used two modules within Python: CSV and OS.  The figure below shows the analysis of the audit data, which provided the results on electoral participation a well as the election winner.  For each piece of data, we will further break down the analysis and highlight the approach used for coding the automation in Python.

>**Congressional Election Results and Participation**

![Congressional Election Results and Participation](https://github.com/MaureenFromuth/Election_Analysis_Challenge/blob/master/Election_Results-Text.png)


- **Question 1: How many votes were cast in this congressional election?**

        Answer: 376,711

To determine the total number of votes (or ballots) cast within this congressional election, we itialized a variable 'total_votes' to count the total votes and set it equal to 0.  After opening the election_results.csv data, we utilized a 'for' loop to move through each vote (row), adding an additional count to the total_votes.  
```
#Election_results data
file_to_load = os.path.join("election_results.csv")

#Establish variable to count total number of votes cast
total_votes = 0

#Open the election_results data
with open(file_to_load) as election_data:
    reader = csv.reader(election_data)

    #Skip past the header of the election_results data
    header = next(reader)

    # For each row in the CSV file ...
    for row in reader:

        # Add to the total vote count
        total_votes = total_votes + 1
```


- **Question 2: Provide a breakdown of the number of votes and the percentage of total votes for each county in the precinct.**

        Answer: Jefferson: 10.5% (38,855); Denver: 82.8% (306,055); Arapahoe: 6.7% (24,801)

To assess the overall particpation of each county, we utilized created one list and one dictionary.  The list established the name of the counties that there were ballots cast, whereas the dictionary established the number of votes per county (key).  
```
#Establish a list for all the counties from which a vote came from
county_list = []

#Establish a dictionary for number of votes (key) per county
county_votes = {}
```

Employing a conditional statement within a for loop that reviewed each row, the code looks for a county not alreaedy in the county names list.  If it is a new county, the code appends the count names list and adds that name, setting the count of votes from that county equal to 0.  Outside of the conditional statement but within the same for loop, every time a county appears on a row another count is added to the total number of ballots from that county.  This gives us the names of all the counties from which we had votes as well as the information needed for the dictionary pairing total number of ballots to the county (key).   
```
with open(file_to_load) as election_data:
    reader = csv.reader(election_data)
    header = next(reader)

    # For each row in the CSV file ...
    for row in reader:

    #The conditional statement for each row that looks for if a county is NOT in the list of existing county names from which a ballot (or vote) was cast
    if county_name not in county_list:

            #If it is NOT in the list, append the list and add the existing county to the list of counties
            county_list.append(county_name)

            #If it is NOT in the list, begin tracking the county's vote count.
            county_votes[county_name] = 0
        
        #For every row in which you see that count name, add a vote to that county's vote count.
        county_votes[county_name] +=1
```

Once these new lists are appended and counted, we used another for loop to consolidate the number of votes per county as well as calculate the percentage of the overall vote that county was responsible for.  The for loop, the code goes through each of the counties (row/key) in the dictionary and pulls out the total count.  Combining this information with the total votes in question one, the code calculates the percentage of votes from each county.
```
    #Goes through each row of the dictionary pairing votes to counties, as calculated above
    for county_name in county_votes:

        # Gets the name of every county (row/key) and gets the votes (variable) associated with that key
        county_ballots = county_votes.get(county_name)

        #For each county in the dictionary, converts the number of ballots and total votes to float and then calculates the percentage 
        county_percentage = float(county_ballots) / float(total_votes) * 100
```


- **Question 3: Which county had the largest number of votes?**

        Answer: Denver 

We first established the three new variables in order to determine the county with the largest number of votes and therefore the largest voter turn out.  These variables identified the county with the largest turn out, the number of votes in that county, and the percentage of overall votes that county had.  
```
#Establish a variable for the name of the county with the largest turnout
largest_turnout_county = ""

#Establish a variable for the number of ballots (or votes) associated with the county with the largest turnout
largest_turnout_ballots = 0

#Establish a variable for the percentage of total votes (or ballots) cast that came from the county with the largest turn out  
largest_turnout_percentage = 0
```

We then used a nested conditional statement within the loop that looped through the dictionary of counties (row/key) and their total votes (variable).  For each county, the conditional statement looked for two conditions to be true: that county had MORE THAN the county with the existing county with the largest votes AND that county has to have A HIGHER percentage of the total votes that the existing county with the top percentage of turnout.  While mathamatically, the largest turn out should also have the largest percentage, this first conditional statement is a good check that your calculations are accurate.  If those two conditions are met, then the county name, number of votes, and percentage of votes replace the existing values in the three variables above.  You can see which county had the largest participation by printing that variable following the loop. 

```
    for county_name in county_votes:

        #If a new county has more votes and a larger percentage of the total vote than the existing county, replace the 'largest' variables.
        if (county_ballots > largest_turnout_ballots) and (county_percentage > largest_turnout_percentage):
            largest_turnout_ballots = county_ballots
            largest_turnout_county = county_name
            largest_turnout_percentage = county_percentage

    #Print off the entry (county) that is assigned to the largest turnout variable after the loop is finished
    print(largest_turnout_county)
```


- **Question 4: Provide a breakdown of the number of votes and the percentage of the total votes each candidate received.**

        Answer: Charles Casper Stockham: 23.0% (85,213); Diana DeGette: 73.8% (272,892); Raymon Anthony Doane: 3.1% (11,606)

Using the exact same approach and much of the same code as above with counties, we established a list of candidates who received votes and a dictionary pairing together those candidates (key) with the total number of votes they received.
```
#Establish a list for all the candidates which received a vote 
candidate_options = []

#Establish a dictionary for number of votes (key) each candidate received 
candidate_votes = {}
```

After opening up the election results data, we wrote a loop that goes through each vote (row), skipping the header.  This loop has a conditional statement within it, much like that of the county analysis above, that looks for a candidates name that is NOT in the list of existing candidate names.  If there is a NEW candidate name, the code appends the candidate list and adds that new name.  At the same time, it also sets the number of votes (variable) assigned to that candidate (key) within the dictionary as 0.  For every time the program runs through a row with a candidate's name, it will add a count to the total number of votes (variable) associated to that candidate (key) in the dictionary.

```
    #The conditional statement for each row that looks for if a candidate is NOT in the list of existing candidate names that received a vote 
    if candidate_name not in candidate_options:

            #If it is NOT in the list, append the list and add the existing canddiate to the list of candidate options
            candidate_options.append(candidate_name)

            #If it is NOT in the list, begin tracking the candidate's vote count.
            candidate_votes[candidate_name] = 0
        
        #For every row in which you see that candidate's name, add a vote to that candidate's vote count.
        candidate_votes[candidate_name] += 1
```

Once these new lists are appended and counted, we used another for loop to consolidate the number of votes per candidate as well as calculate the percentage of the overall vote that candidate received.  With the for loop, the program goes through each of the candidates (row/key) in the dictionary and pulls out the total count.  Combining this information with the total votes in question one, the code calculates the percentage of votes for each candidate.

```
    #Goes through each row of the dictionary pairing votes to candidates, as calculated above
    for candidate_name in candidate_votes:

        # Gets the name of every candidate (row/key) and gets the votes (variable) associated with that key
        votes = candidate_votes.get(candidate_name)

        #For each candidate in the dictionary, converts the number of total votes to float and then calculates the percentage 
        vote_percentage = float(votes) / float(total_votes) * 100
```

- **Question 5: Which candidate won the election, what was their vote count, and what was their percentage of the total votes?**

        Answer:  Winner: Diana DeGette; Winning Vote Count: 272,892; Winning Percentage: 73.8%

Yet again, the majority of this code and the approach to building it is the same as that of Question 3.  Much like the analysis of county turn out, we first established three additiohnal variables and set them to 0.  These variables were assigned to the winning candidate, the total number of votes for that winning candidat, and the percentage of overall vote that candidate received.
```
#Establish a variable for the winning candidate's name 
winning_candidate = ""

#Establish a variable for the total number of votes the winning candidate received 
winning_count = 0

#Establish a variable for the percentage of overall votes the winning candidate received 
winning_percentage = 0
```

Using the nested conditional approach listed in Question 3, we use a loop to go through each candidate name within the dictionary and look for candidates that have a higher number of votes and a higher percentage of the overall vote that the candidate currently identified as the 'winner.  If there is a candidate (row/key) that meets both of those conditions, we replace the existing values with the new values for these three variables.
```
    for candidate_name in candidate_votes:

        #If a new candidate has more votes and a larger percentage of the total vote than the existing winner, replace the 'winning' variables.
        if (votes > winning_count) and (vote_percentage > winning_percentage):
            winning_count = votes
            winning_candidate = candidate_name
            winning_percentage = vote_percentage
```

## Summary
Assuming each county's data has the same or similar structure, this code can be reused for multiple elections.  There are a few key areas you would need to update or change in order for this to occur, however.  The first would be the location of the data you are loading to review and the location of the data that you would write the results to.  Using the OS module path.join feature, you would need to assign the location of the new data as well as identify the location of your output text file.  Furthermore, you need to identify that the rows and headers are in the same order.  If not, you can either adjust the results data or you can adjust the code to look at the appropriate row (e.g. row 1 is now county).  If there are concurrent elections and all the data is co-lotated into one results tabular data sheet, you would need to add additional indexes to associated specific counties to a particular election. 

To identify more specific examples of how to resuse this code, let's look at two different scenarios: same data new election and district; different data same election and district.  

First, let's look at a new election using the same data structure but for an election with different candidates and different counties.  In this case, you would only need to change the location and name of the files you are loading for analysis and the output file.  See below for an example.
```
# Add a variable to load a file from a path.
file_to_load = os.path.join("ADD NEW NAME OF ELECTION DATA HERE.csv")

# Add a variable to save the file to a path.
file_to_save = os.path.join("ADD NEW NAME OF OUTPUT OF ELECTION ANALYSIS HERE.txt")
```

Another example is if we get an additional attribute or column (e.g. voter age range) per ballot for the exact same election and the exact same district.  If, Tom and Seth are now interested in understanding the age demographics of the voters (e.g. 18-30, 30-50, 50+), you can employ the exact same approach as we did with counties and candidates to assess the age group with the largest turn out.  More specifically, you first establish a list of age groups, and establish a dictionary that assigns votes (variables) to the age groups (keys). Using a for loop, you go through each of the votes (rows) in the newly updated election data, adding age groups to the possible age group list and adding one more count for each age group the program comes across.  Using another for loop but this time within the list of age groups, you get the total votes for each age group and use that data to calculate the percentage of votes each age group received.  Finally, using a nested conditional statement within that same loop, you look for age groups with more votes and a higher percentage of the overall votes than the existing 'winner'.  For any age group higher, replace the fields for the respective variables.
