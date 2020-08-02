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


Provide a breakdown of the number of votes and the percentage of total votes for each county in the precinct.
Which county had the largest number of votes?
Provide a breakdown of the number of votes and the percentage of the total votes each candidate received.
Which candidate won the election, what was their vote count, and what was their percentage of the total votes?


## Summary

