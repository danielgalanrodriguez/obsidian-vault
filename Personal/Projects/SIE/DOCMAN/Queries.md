
# Get groups with more than 2 facilities:

Only considers de default group. Set in the user table(u.sie_facility_id) :
```sql
SELECT g.id,g.name,COUNT(DISTINCT u.sie_facility_id ) as differentFacilites
FROM `groups` g 
INNER JOIN
users u ON g.id = u.group_id
GROUP BY
g.id, g.name
HAVING
COUNT(DISTINCT u.sie_facility_id) >= 2;
```


Considering 'user-groups' as well:`
`
```sql
SELECT
    g.id AS group_id,
    g.name AS group_name,
    COUNT(DISTINCT u.sie_facility_id) AS distinct_facility_count
FROM
    `groups` g
INNER JOIN
    (
        SELECT
            id AS user_id,
            group_id
        FROM
            users
        WHERE
            group_id IS NOT NULL

        UNION

        SELECT
            user_id,
            group_id
        FROM
            user_groups
    ) AS uga ON g.id = uga.group_id
INNER JOIN
    users u ON uga.user_id = u.id
GROUP BY
    g.id, g.name
HAVING
    COUNT(DISTINCT u.sie_facility_id) >= 2;
```
![[db-examples.png]]