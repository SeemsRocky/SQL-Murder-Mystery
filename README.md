# SQL-Murder-Mystery

##Experienced SQL sleuths start here
A crime has taken place and the detective needs your help. The detective gave you the crime scene report, but you somehow lost it. You vaguely remember that the crime was a ​murder​ that occurred sometime on ​Jan.15, 2018​ and that it took place in ​SQL City​. Start by retrieving the corresponding crime scene report from the police department’s database.

```
SELECT description
FROM crime_scene_report
WHERE city="SQL City"
AND date="20180115"
AND type = "murder"
```
description|
-----|
Security footage shows that there were 2 witnesses. The first witness lives at the last house on "Northwestern Dr". The second witness, named Annabel, lives somewhere on "Franklin Ave".	|

```
SELECT *
FROM person
WHERE (address_street_name="Northwestern Dr"
AND address_number = (SELECT MAX(address_number) FROM person LIMIT 1))
OR (name LIKE "%Annabel%" AND address_street_name="Franklin Ave")
```

id|	name|	license_id|	address_number|	address_street_name|	ssn|
---|----|-----------|---------------|-------------------|---------|
14887| Morty Schapiro |118009 |	4919|	Northwestern Dr|	111564949|
16371|	Annabel Miller|	490173|	103	|Franklin Ave|	318771143|

```
SELECT transcript 
FROM interview 
WHERE person_id IN(14887,16371) 
```

transcript|
----|
I heard a gunshot and then saw a man run out. He had a "Get Fit Now Gym" bag. The membership number on the bag started with "48Z". Only gold members have those bags. The man got into a car with a plate that included "H42W".|
I saw the murder happen, and I recognized the killer from my gym when I was working out last week on January the 9th.|

```
SELECT p.name
FROM person p
LEFT JOIN get_fit_now_member m ON (p.id = m.person_id)
WHERE m.id IN ( SELECT membership_id 
		      FROM get_fit_now_check_in 
		      WHERE check_in_date = "20180109"
		      AND membership_id LIKE "48Z%")
AND p.license_id IN (SELECT id 
			FROM drivers_license 
			WHERE plate_number LIKE "%H42W%")
```

name|
----|
Jeremy Bowers|

```
INSERT INTO solution VALUES (1, 'Jeremy Bowers');
        
        SELECT value FROM solution;
```
value|
----|
Congrats, you found the murderer! But wait, there's more... If you think you're up for a challenge, try querying the interview transcript of the murderer to find the real villian behind this crime. If you feel especially confident in your SQL skills, try to complete this final step with no more than 2 queries. Use this same INSERT statement to check your answer.|

```
SELECT transcript 
FROM interview 
WHERE person_id = 67318
```
transcript|
------|
I was hired by a woman with a lot of money. I don't know her name but I know she's around 5'5" (65") or 5'7" (67"). She has red hair and she drives a Tesla Model S. I know that she attended the SQL Symphony Concert 3 times in December 2017.|

```
SELECT p.name
FROM person p
WHERE p.id IN (SELECT person_id
			   FROM facebook_event_checkin
			   WHERE event_name = "SQL Symphony Concert"
			   AND date LIKE ("201712%")
			   GROUP BY person_id
			   HAVING COUNT(person_id) = 3)
AND p.license_id IN (SELECT id
					  FROM drivers_license
					  WHERE car_make = "Tesla"
					  AND car_model = "Model S"
					  AND height BETWEEN 65 AND 67
					  AND gender = "female"
					  AND hair_color = "red")
```

name |
-----|
Miranda Priestly|

```
INSERT INTO solution VALUES (1, 'Miranda Priestly');
        
        SELECT value FROM solution;
```
value|
----|
Congrats, you found the brains behind the murder! Everyone in SQL City hails you as the greatest SQL detective of all time. Time to break out the champagne!|
