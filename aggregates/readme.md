## (Data aggregation)[https://pgexercises.com/questions/aggregates]

#### Count the number of facilities
SELECT count(*) FROM cd.facilities;

#### Count the number of expensive facilities
SELECT count(*) FROM cd.facilities 
WHERE guestcost >= 10;

#### Count the number of recommendations each member makes.
select recommendedby, count(*) 
	from cd.members
	where recommendedby is not null
	group by recommendedby
order by recommendedby;          

#### List the total slots booked per facility
SELECT facid, sum(slots) as "Total Slots"
FROM cd.bookings
GROUP BY facid
ORDER BY facid;

#### List the total slots booked per facility in a given month
select facid, sum(slots) as "Total Slots"
	from cd.bookings
	where
		starttime >= '2012-09-01'
		and starttime < '2012-10-01'
	group by facid
order by sum(slots);          

#### List the total slots booked per facility per month
SELECT facid, extract(month from starttime) as month, sum(slots) as "Total Slots"
FROM cd.bookings
WHERE extract (year from starttime) = 2012
GROUP BY facid, month
ORDER BY facid, month;

#### Find the count of members who have made at least one booking
SELECT count(distinct memid) FROM cd.bookings;

#### List facilities with more than 1000 slots booked
SELECT facid, sum(slots) as "Total Slots"
FROM cd.bookings
GROUP BY facid
having sum(slots) > 1000
ORDER BY facid

#### Output the facility id that has the highest number of slots booked
select facid, sum(slots) as "Total Slots"
	from cd.bookings
	group by facid
order by sum(slots) desc
LIMIT 1; 
#### Produce a list of member names, with each row containing the total member count:
select count(*) over(), firstname, surname
	from cd.members
order by joindate     
