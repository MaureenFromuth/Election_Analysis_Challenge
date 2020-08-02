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


- **Question: How many votes were cast in this congressional election?**

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

- **Question: Provide a breakdown of the number of votes and the percentage of total votes for each county in the precinct.**

        Answer: Jefferson: 10.5% (38,855); Denver: 82.8% (306,055); Arapahoe: 6.7% (24,801)
To assess the overall particpation of each county, we utilized created one list, one dictionary, and 3 new variables.  The list established the name of the counties that there were ballots cast, whereas the dictionary established the number of votes per county (key).  The three remaining variables identified the county with the largest turn out, the number of votes in that county, and the percentage of overall votes that county had.  Employing a conditional statement within a for loop that reviewed each row, the code looks for a county not alreaedy in the county names list.  If it is a new county, the code appends the count names list and adds that name, setting the count of votes from that county equal to 0.  Outside of the conditional statement but within the same for loop, every time a county appears on a row another count is added to the total number of ballots from that county.  This gives us the names of all the counties from which we had votes as well as the total number of ballots per county.   
```
#Establish a list for all the counties from which a vote came from
county_list = []

#Establish a dictionary for number of votes (key) per county
county_votes = {}

#Establish a variable for the name of the county with the largest turnout
largest_turnout_county = ""

#Establish a variable for the number of ballots (or votes) associated with the county with the largest turnout
largest_turnout_ballots = 0

#Establish a variable for the percentage of total votes (or ballots) cast that came from the county with the largest turn out  
largest_turnout_percentage = 0

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

- **Question: Which county had the largest number of votes?**

```
```

- **Question: Provide a breakdown of the number of votes and the percentage of the total votes each candidate received.**

```
```

- **Question: Which candidate won the election, what was their vote count, and what was their percentage of the total votes?**

```
```

## Summary

how can be used for other Elections

2 examples
 - switch out names/counties (if data is the same; need to change out the location of the data in the file_to_load)
 - switch out the attribute assessing (create new variables similar to that of name and county, but create new row)
