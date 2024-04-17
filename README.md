# IPL-Bidding-Blitz-Dive-into-the-Action-_SQL-project
Wow! Check out my Pie-in-the-Sky project, where you dive into the thrilling world of IPL bidding! Make predictions, place your bets, and rack up points in a risk-free environment. It's your chance to top the leader board and experience the excitement of each match. Explore now and join the action! 🏏✨

# Tech Stack used
<img src="https://github.com/Abdulmalik25/Travego_Travelers_Sql--Project/assets/153974173/fe50efa7-c836-4213-b20f-0b89ad397eb9" alt="images" width="450" height="350">

# Introduction
Pie-in-the-Sky is an exhilarating mobile app where IPL fans can legally bid on matches! Users register with basic contact info and plunge into the thrill of predicting IPL match winners before the toss. Updated in real time with match details, team standings, and user rankings, the app ensures you're at the heart of the action. Bid, predict, and accumulate points without any risk of losing them. Exciting, right? Get ready to challenge your predictions and climb the leader board!

# Data Model
## List of Tables
1. IPL_User
2. IPL_Stadium
3. IPL_Team
4. IPL_Player
5. IPL_Team_Players
6. IPL_Tournament
7. IPL_Match
8. IPL_Match_Schedule
9. IPL_Bidder_details
10. IPL_Biddding_Details
11. IPL_Bidder_points
12. IPL_Team_Standings

## ER- Diagram
![ipl](https://github.com/Abdulmalik25/IPL-Bidding-Blitz-Dive-into-the-Action-_SQL-project/assets/153974173/3885ede5-4b99-4963-ab0a-c3ac8099dc12)

# Getting Started
## Database Creation
Step-1  Involves creation of 12 Diffrent tables in the databaase named IPL.
 After creation of table values has been inserted for analysis.

## Analytical Queries
1. Show the percentage of wins of each bidder in the order of highest to lowest
percentage.
``` sql
select bidder_name,
       count(if(bid_status = 'won',1,null)) as won,
       count(A.BIDDER_ID) as total,
       round(count(if(bid_status = 'won',1,null))/count(A.bidder_id)*100,2) as win_percentage
from IPL_BIDDING_DETAILS A inner join IPL_BIDDER_DETAILS B
on A.BIDDER_ID = B.BIDDER_ID
group by A.BIDDER_ID
order by win_percentage desc;
```
![3a](https://github.com/Abdulmalik25/IPL-Bidding-Blitz-Dive-into-the-Action-_SQL-project/assets/153974173/8e208e67-5c4c-4cda-9e85-1fd4f49f67f2)


2. Display the number of matches conducted at each stadium with the stadium name and city.
``` sql
select STADIUM_NAME, CITY, count(*) as no_of_matches
from IPL_MATCH A inner join IPL_MATCH_SCHEDULE B inner join IPL_STADIUM C
on A.MATCH_ID = B.MATCH_ID and B.STADIUM_ID = C.STADIUM_ID
group by C.STADIUM_ID;
```
![3b](https://github.com/Abdulmalik25/IPL-Bidding-Blitz-Dive-into-the-Action-_SQL-project/assets/153974173/e41ba91d-826d-48bd-859f-276da441edaf)

3. In a given stadium, what is the percentage of wins by a team which has won the
toss?
``` sql
select C.STADIUM_NAME,
       count(if(match_winner = TOSS_WINNER, 1, null)) as won,
       count(*) as total,
       round((count(if(MATCH_WINNER = TOSS_WINNER,1,null))/count(*))*100,2) as won_percentage
from IPL_MATCH A inner join IPL_MATCH_SCHEDULE B inner join IPL_STADIUM C
on A.MATCH_ID = B.MATCH_ID and B.STADIUM_ID = C.STADIUM_ID
group by C.STADIUM_ID;
```
![3c](https://github.com/Abdulmalik25/IPL-Bidding-Blitz-Dive-into-the-Action-_SQL-project/assets/153974173/7cfb90af-862b-402b-b3f0-fbb59658733a)

4. Show the total bids along with the bid team and team name.
``` sql
select A.bid_team, B.TEAM_NAME, count(*) as total_bids
from IPL_BIDDING_DETAILS A inner join IPL_TEAM B
on A.BID_TEAM = B.TEAM_ID
group by A.BID_TEAM;
```
![3d](https://github.com/Abdulmalik25/IPL-Bidding-Blitz-Dive-into-the-Action-_SQL-project/assets/153974173/a076edfe-9abb-448c-8288-d6db7c089a2f)

5. Show the team id who won the match as per the win details.
``` sql
select TEAM_ID, TEAM_NAME,WIN_DETAILS
from IPL_MATCH A inner join IPL_TEAM B
on trim(left(substr(WIN_DETAILS,6),position(' ' in substr(win_details,6)))) = B.REMARKS; -- won the match as per win_details;
```
![3e](https://github.com/Abdulmalik25/IPL-Bidding-Blitz-Dive-into-the-Action-_SQL-project/assets/153974173/ce00d570-a645-4eb5-9d8c-1a4ee51cba33)


6. Display total matches played, total matches won and total matches lost by team
along with its team name.
``` sql
select B.TEAM_NAME, sum(MATCHES_WON) as won, sum(MATCHES_LOST) as lost, sum(MATCHES_PLAYED) as total
from IPL_TEAM_STANDINGS A inner join IPl_TEAM B
on A.TEAM_ID = B.TEAM_ID
group by A.TEAM_ID;
```
![3f](https://github.com/Abdulmalik25/IPL-Bidding-Blitz-Dive-into-the-Action-_SQL-project/assets/153974173/5c1e13c5-6c7f-4130-8164-7d147edaeb7d)

7. Display the bowlers for Mumbai Indians team
``` sql
select team_id, A.PLAYER_ID, PERFORMANCE_DTLS
from IPL_TEAM_PLAYERS A inner join IPL_PLAYER B
on A.PLAYER_ID = B.PLAYER_ID and A.REMARKS like '%MI%' and A.PLAYER_ROLE like '%Bowler%';
```
![3g](https://github.com/Abdulmalik25/IPL-Bidding-Blitz-Dive-into-the-Action-_SQL-project/assets/153974173/35375d57-f647-4a1e-a026-1e82f59f2630)

8. How many all-rounders are there in each team, Display the teams with more than 4 all-rounders in descending order.
``` sql
select replace(REMARKS,'TEAM - ','') as REMARKS, count(*) as total
from IPL_TEAM_PLAYERS
where PLAYER_ROLE like '%All-Rounder%'
group by REMARKS
having total > 4
order by total desc;
```
![3h](https://github.com/Abdulmalik25/IPL-Bidding-Blitz-Dive-into-the-Action-_SQL-project/assets/153974173/a340ae46-6a6e-42f3-a3fd-9cd2b13ebe23)

