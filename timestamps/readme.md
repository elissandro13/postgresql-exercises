## (Working with Timestamps)[https://pgexercises.com/questions/date/]

#### Produce a timestamp for 1 a.m. on the 31st of August 2012
SELECT timestamp '2012-08-31 01:00:00';   

#### Subtract timestamps from each other
SELECT timestamp '2012-08-31 01:00:00' - timestamp '2012-07-30 01:00:00' as interval;  

#### Generate a list of all the dates in October 2012
SELECT generate_series(timestamp '2012-10-01', timestamp '2012-10-31', interval '1 day') as ts;  

#### Get the day of the month from a timestamp
SELECT extract(day FROM timestamp '2012-08-31')

#### Work out the number of seconds between timestamps
select 	extract(day from ts.int)*60*60*24 +
	extract(hour from ts.int)*60*60 + 
	extract(minute from ts.int)*60 +
	extract(second from ts.int)
	from
		(select timestamp '2012-09-02 00:00:00' - '2012-08-31 01:00:00' as int) ts

#### Work out the number of days in each month of 2012
select 	extract(month from cal.month) as month,
	(cal.month + interval '1 month') - cal.month as length
	from
	(
		select generate_series(timestamp '2012-01-01', timestamp '2012-12-01', interval '1 month') as month
	) cal
order by month;          

#### Work out the number of days remaining in the month
select (date_trunc('month',ts.testts) + interval '1 month') 
		- date_trunc('day', ts.testts) as remaining
	from (select timestamp '2012-02-11 01:00:00' as testts) ts 

#### Work out the end time of bookings
SELECT starttime, starttime + slots* (interval '30 minutes') as endtime
FROM  cd.bookings
ORDER BY  endtime DESC, starttime DESC
LIMIT 10;

#### Return a count of bookings for each month
SELECT date_trunc('month', starttime) As month, count(*)
FROM cd.bookings
group by month
order by month

#### Work out the utilisation percentage for each facility by month
select name, month, 
	round((100*slots)/
		cast(
			25*(cast((month + interval '1 month') as date)
			- cast (month as date)) as numeric),1) as utilisation
	from  (
		select facs.name as name, date_trunc('month', starttime) as month, sum(slots) as slots
			from cd.bookings bks
			inner join cd.facilities facs
				on bks.facid = facs.facid
			group by facs.facid, month
	) as inn
order by name, month;       
