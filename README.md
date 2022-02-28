# RDS-Project-1
Database of Pokemon created using MS Access. Includes Forms, Reports, SQL Queries, as well as one-to-one and one-to-many relationships
## Relationships
 1. Pokemon
	 -   One to Many relation between Gyms, name -> partnerName, essentially every gym has a gym leader who has a partner pokemon which can be found in our Pokemon table.
    - One to Many relation between Types, TypeID -> ID, every pokemon can have 1-2 types and various multipliers against other types.
    - One to Many relation between Stats, statID -> ID, every pokemon has stats such as attack, defense, etc, which is related to in the stats table.
 2. Gym
	- One to One relation between Towns, Each town can possibly have a gym located there.
	- One to Many relation between Regions, each region can have multiple gyms
 3. Region
	- One to Many relation between Towns, Each town is in a certain region.
## Database Design
**![](https://lh5.googleusercontent.com/c3N8IXZK4cWBOdONyriRQgmvvqEBZnTC9_WXQ1XHrKgNd-OnkhTgK8-EIhUEN7bkB0cjr6yarWq1o6la4xwwuHF7ZJBR4qJbIVyR9_Vkgqy9oLW5b_FOM37m2w8wIYY2t14ISfl0OD4)**
## Queries
 1. **What Pokemon exists in the water gym, and what is its types?**
	 - This query would be useful to create a strategy against the water gym. If you know exactly which pokemon exist in the gym, you would be able to preemptively make a team which easily counters them.
	- ```SQL
SELECT Pokemon.name, Types.type1, Types.type2
FROM Pokemon, [Gym Leaders], Types
WHERE [Gym Leaders].partnerName = Pokemon.name
AND Pokemon.TypeID = Types.ID
AND [Gym Leaders].ID = 2
```
2. **What are the second generation pokemon weak to grass types**
	- This query is useful to determine if a pokemon weak to grass and fire types exists in a specific generation.
	- ```SQL
SELECT Pokemon.name
FROM Pokemon, Types
WHERE Pokemon.typeID = Types.ID
AND Types.againstGrass < 1
AND Types.againstFire < 1
AND Pokemon.generation = 2```
3. **What are the towns in Kanto, and what is that town’s Gym badge**
	- This query would be useful to determine which gym badges exist in each town in a specific region, in this case, Kanto.
	- ```SQL
SELECT Town.name, [Gym Leaders].badgeName
FROM Town, [Gym Leaders]
WHERE Town.regionID = 1
AND Town.GymID = [Gym Leaders].ID```
4. **Which pokemon have the highest average damage of types grass and ground**
	- This query would be useful when drafting a team of pokemon. Say you already have other areas of combat covered, meaning your team already has good counters to fire, water, ice, etc. But you notice a weakness in the grass and ground category, with this query, you’d be able to very quickly see what your best option is to patch this weakness.
	- ```SQL
SELECT p.name, (s.attack + s.spAttack) / 2 as AvgAttack
FROM (Pokemon AS p
INNER JOIN Stats AS s ON p.statID = s.ID)
INNER JOIN Types AS t ON p.TypeID = t.ID
WHERE t.type1 IN ('grass', 'ground')
AND t.type2 IN ('grass', 'ground')
ORDER BY (s.attack + s.spAttack) / 2 DESC```
5. **How many pokemon are weak to fire in each generation**
	- This query returns the number of pokemon weak to the fire type per generation. It would be useful to determine if fire is a powerful type for each generation.
	- ```SQL
SELECT Pokemon.generation, COUNT(Pokemon.ID) as Number_of_Pokemon
FROM Pokemon
INNER JOIN Types ON Pokemon.TypeID = Types.ID
WHERE againstFire < 1
GROUP BY Pokemon.generation```
