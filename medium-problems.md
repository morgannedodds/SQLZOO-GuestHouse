

### 6. Ruth Cadbury. Show the total amount payable by guest Ruth Cadbury for her room bookings. You should JOIN to the rate table using room_type_requested and occupants.

```SQL
select sum(b.nights*r.amount)
from booking b
inner join rate r on b.room_type_requested = r.room_type and b.occupants = r.occupancy
inner join guest g on b.guest_id = g.id
where g.first_name = 'Ruth' and g.last_name = 'Cadbury'
```

### 7. Including Extras. Calculate the total bill for booking 5346 including extras.

```SQL
with e as(
select booking_id, sum(amount) as amount from extra
group by 1) 

select (sum(b.nights*r.amount) + sum(e.amount)) as amount
from booking b
inner join rate r on b.occupants = r.occupancy and b.room_type_requested = r.room_type
inner join e
on b.booking_id = e.booking_id
where b.booking_id = 5346
```

### 8. Edinburgh Residents. For every guest who has the word “Edinburgh” in their address show the total number of nights booked. Be sure to include 0 for those guests who have never had a booking. Show last name, first name, address and number of nights. Order by last name then first name.

```SQL
select g.last_name, g.first_name, g.address, sum(case when b.nights is null then 0 else b.nights end) as nights
from booking b 
right join guest g on b.guest_id = g.id 
where g.address like '%Edinburgh%'
group by 1, 2, 3
order by 1, 2
```
