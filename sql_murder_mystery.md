### Get murder crime scene reports for SQL City
`SELECT * FROM crime_scene_report WHERE type = 'murder' AND city = 'SQL City';`

date description
20180215 (Sat Aug 22 1970 13:36:55 GMT+0000) | REDACTED REDACTED REDACTED
20180215 (Sat Aug 22 1970 13:36:55 GMT+0000) | Someone killed the guard! He took an arrow to the knee!
20180115 (Sat Aug 22 1970 13:35:15 GMT+0000) | Security footage shows that there were 2 witnesses. The first witness lives at the last house on "Northwestern Dr". The second witness, named Annabel, lives somewhere on "Franklin Ave".

---

### Follow up on witness Annabel
`SELECT * FROM person WHERE name LIKE '%Annabel%';`

id name license_id address_number address_street_name ssn
16371 Annabel Miller 490173 103 Franklin Ave 318771143
(3 other rows)

`SELECT * FROM interview WHERE person_id = 16371;`
person_id transcript
16371 I saw the murder happen, and I recognized the killer from my gym when I was working out last week on January the 9th.

`SELECT * FROM get_fit_now_check_in WHERE check_in_date = 20180109;`
`SELECT * FROM get_fit_now_check_in WHERE check_in_date = 20180109 AND check_in_time <= 1600 AND check_out_time >= 1700 AND NOT membership_id = '90081';`
membership_id check_in_date check_in_time check_out_time
X0643 20180109 957 1164
UK1F2 20180109 344 518
XTE42 20180109 486 1124
1AE2H 20180109 461 944
6LSTG 20180109 399 515
7MWHJ 20180109 273 885
GE5Q8 20180109 367 959
48Z7A 20180109 1600 1730 <--
48Z55 20180109 1530 1700 <--
90081 20180109 1600 1700 <-- Annabel

`SELECT * FROM get_fit_now_member WHERE person_id = 16371;`
id person_id name membership_start_date membership_status
90081 16371 Annabel Miller 20160208 gold

`SELECT * FROM get_fit_now_member WHERE id IN ('48Z7A', '48Z55');`
id person_id name membership_start_date membership_status
48Z55 67318 Jeremy Bowers 20160101 gold
48Z7A 28819 Joe Germuska 20160305 gold

---

`SELECT * FROM person WHERE address_street_name = 'Northwestern Dr' ORDER BY address_number DESC LIMIT 1;`

id name license_id address_number address_street_name ssn
14887 Morty Schapiro 118009 4919 Northwestern Dr 111564949

`SELECT * FROM interview WHERE person_id = 14887;`
person_id transcript
14887 I heard a gunshot and then saw a man run out. He had a "Get Fit Now Gym" bag. The membership number on the bag started with "48Z". Only gold members have those bags. The man got into a car with a plate that included "H42W".

`SELECT * FROM person WHERE id IN (67318, 28819);`
id name license_id address_number address_street_name ssn
28819 Joe Germuska 173289 111 Fisk Rd 138909730
67318 Jeremy Bowers 423327 530 Washington Pl, Apt 3A 871539279

`SELECT * FROM person JOIN drivers_license ON person.license_id = drivers_license.id WHERE person.id IN (67318, 28819);`
id name license_id address_number address_street_name ssn id age height eye_color hair_color gender plate_number car_make car_model
67318 Jeremy Bowers 423327 530 Washington Pl, Apt 3A 871539279 423327 30 70 brown brown male 0H42W2 Chevrolet Spark LS

---

`SELECT * FROM interview WHERE person_id = 67318;`
person_id transcript
67318 I was hired by a woman with a lot of money. I don't know her name but I know she's around 5'5" (65") or 5'7" (67"). She has red hair and she drives a Tesla Model S. I know that she attended the SQL Symphony Concert 3 times in December 2017.

`SELECT * FROM person JOIN drivers_license ON person.license_id = drivers_license.id WHERE hair_color = 'red' AND gender = 'female' AND height BETWEEN 64 AND 68 AND car_make = 'Tesla' AND car_model = 'Model S';`
id name license_id address_number address_street_name ssn id age height eye_color hair_color gender plate_number car_make car_model
78881 Red Korb 918773 107 Camerata Dr 961388910 918773 48 65 black red female 917UU3 Tesla Model S
90700 Regina George 291182 332 Maple Ave 337169072 291182 65 66 blue red female 08CM64 Tesla Model S
99716 Miranda Priestly 202298 1883 Golden Ave 987756388 202298 68 66 green red female 500123 Tesla Model S

`SELECT * FROM facebook_event_checkin WHERE person_id IN (78881, 90700, 99716);`
person_id event_id event_name date
99716 1143 SQL Symphony Concert 20171206
99716 1143 SQL Symphony Concert 20171212
99716 1143 SQL Symphony Concert 20171229
