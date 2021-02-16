[Modifying data](https://pgexercises.com/questions/updates/)

#### Insert some data into a table
insert into cd.facilities (facid, name, membercost, guestcost, initialoutlay, monthlymaintenance)
values (9, 'Spa', 20, 30, 100000, 800);  

#### Insert multiple rows of data into a table
INSERT INTO cd.facilities
    (facid, name, membercost, guestcost, initialoutlay, monthlymaintenance)
    VALUES
        (9, 'Spa', 20, 30, 100000, 800),
        (10, 'Squash Court 2', 3.5, 17.5, 5000, 80);       

#### Insert calculated data into a table

INSERT INTO cd.facilities
    (facid, name, membercost, guestcost, initialoutlay, monthlymaintenance)
    SELECT (select max(facid) FROM cd.facilities)+1, 'Spa', 20, 30, 100000, 800;     
#### Update some existing data
UPDATE cd.facilities
    SET initialoutlay = 10000
    WHERE facid = 1; 

#### Update multiple rows and columns at the same time
UPDATE cd.facilities
    SET
        membercost = 6,
        guestcost = 30
    WHERE facid in (0,1);          

#### Delete all bookings
DELETE FROM cd.bookings;

#### Delete a member from the cd.members table
DELETE FROM cd.members 
WHERE memid = 37;

#### Delete based on a subquery
DELETE FROM cd.members WHERE memid NOT IN (SELECT memid FROM cd.bookings);

#### Update a row based on the contents of another row
update cd.facilities facs
    set
        membercost = (select membercost * 1.1 from cd.facilities where facid = 0),
        guestcost = (select guestcost * 1.1 from cd.facilities where facid = 0)
    where facs.facid = 1;    

