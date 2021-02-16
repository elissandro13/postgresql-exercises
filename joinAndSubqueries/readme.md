(JOIN-Subqueries)[https://pgexercises.com/questions/joins/]

#### How can you produce a list of the start times for bookings by members named 'David Farrell'?
SELECT starttime from cd.bookings
JOIN cd.members m ON
cd.bookings.memid = m.memid
WHERE m.firstname = 'David' AND m.surname = 'Farrell'

#### How can you produce a list of the start times for bookings for tennis courts, for the date '2012-09-21'? Return a list of start time and facility name pairings, ordered by the time.
SELECT starttime as start, name FROM  cd.bookings b
JOIN cd.facilities f ON
b.facid = f.facid
WHERE f.name LIKE '%Tennis Court%' AND
b.starttime  >= '2012-09-21' AND
b.starttime < '2012-09-22'

#### How can you output a list of all members who have recommended another member? Ensure that there are no duplicates in the list, and that results are ordered by (surname, firstname).
SELECT DISTINCT r.firstname as firstname, r.surname as surname FROM cd.members m
JOIN cd.members r 
on r.memid = m.recommendedby
order by surname, firstname;

#### How can you output a list of all members, including the individual who recommended them (if any)? Ensure that results are ordered by (surname, firstname).
SELECT mem.firstname AS memfname, mem.surname AS memsname, 
rec.firstname AS recfname, rec.surname AS recsname FROM cd.members mem
left outer JOIN cd.members rec ON
rec.memid = mem.recommendedby
ORDER BY memsname, memfname;

#### How can you produce a list of all members who have used a tennis court? Include in your output the name of the court, and the name of the member formatted as a single column. Ensure no duplicate data, and order by the member name followed by the facility name.
SELECT DISTINCT mem.firstname || ' ' || mem.surname AS member, f.name AS facility
FROM cd.members mem 
JOIN cd.bookings b ON
mem.memid = b.memid
JOIN cd.facilities f ON
f.facid = b.facid
WHERE f.name IN ('Tennis Court 1','Tennis Court 2')
ORDER By member, facility;

#### How can you produce a list of bookings on the day of 2012-09-14 which will cost the member (or guest) more than $30? Remember that guests have different costs to members (the listed costs are per half-hour 'slot'), and the guest user is always ID 0. Include in your output the name of the facility, the name of the member formatted as a single column, and the cost. Order by descending cost, and do not use any subqueries.
SELECT m.firstname || ' ' || m.surname as member, f.name as facility,
	case
			when m.memid = 0 then
					b.slots*f.guestcost
				else
					b.slots*f.membercost
		end as cost
		from
			cd.members m
			join cd.bookings b ON
			m.memid = b.memid
			join cd.facilities f ON
			f.facid = b.facid
			
		WHERE b.starttime >= '2012-09-14' and 
		b.starttime < '2012-09-15' and (
			(m.memid = 0 and b.slots*f.guestcost > 30) or
			(m.memid != 0 and b.slots*f.membercost > 30)
		)
order by cost desc;          
#### Produce a list of all members, along with their recommender, using no joins.
SELECT DISTINCT mem.firstname || ' ' || mem.surname as member, 
		(SELECT rec.firstname || ' ' || rec.surname
		 	FROM cd.members rec
		 	WHERE rec.memid = mem.recommendedby
		 )
		FROM cd.members mem
ORDER BY member;	
#### Produce a list of costly bookings, using a subquery
SELECT member, facility, cost FROM (
	SELECT
  		mem.firstname || ' ' || mem.surname AS member,
  		f.name AS facility,
  		case
  			when mem.memid = 0 then
  				b.slots*f.guestcost
  			else
  				b.slots*f.membercost
  		end as cost
  		FROM
  			cd.members mem
  			JOIN cd.bookings b
  				ON mem.memid = b.memid
  			JOIN  cd.facilities f ON
  				b.facid = f.facid
  		WHERE
  			b.starttime >= '2012-09-14' and
			b.starttime < '2012-09-15'
  		) as bookings
		WHERE cost > 30
ORDER BY cost desc;
  			
  
  
  	
  			
			
