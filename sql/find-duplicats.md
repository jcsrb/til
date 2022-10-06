# Find Duplicate Rows based on multiple columns
and display also the ids as a colleciton

PG:
```SQL
select event_date, event_type, event_name ,count(*), array_agg(id) as "event_ids"
from magic_events
where event_type = 'birthday-party' and event_date > now()
group by event_date, event_type, event_name
having count(*) > 1;
```
