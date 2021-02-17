## (String Operation)[https://pgexercises.com/questions/string/]

#### Format the names of members
SELECT surname || ', ' || firstname AS name FROM cd.members;

#### Find facilities by a name prefix
SELECT * FROM cd.facilities
WHERE name LIKE 'Tennis%';

#### Perform a case-insensitive search
SELECT * FROM cd.facilities WHERE upper(name) LIKE 'TENNIS%'; 

#### Find telephone numbers with parentheses
SELECT memid, m.telephone FROM cd.members m
WHERE telephone ~ '[()]'

#### Pad zip codes with leading zeroes
select lpad(cast(zipcode as char(5)),5,'0') zip from cd.members order by zip   

#### Count the number of members whose surname starts with each letter of the alphabet
select substr (mems.surname,1,1) as letter, count(*) as count 
    from cd.members mems
    group by letter
    order by letter          

#### Clean up telephone numbers
SELECT memid, regexp_replace(telephone, '[^0-9]', '', 'g') as telephone
    from cd.members
    order by memid;


