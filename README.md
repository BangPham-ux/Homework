#Tạo thêm id cho các email bị trùng lặp
```
SELECT
  ROWID AS id,
  ROW_NUMBER() OVER (PARTITION BY email ORDER BY DATE(membership_date)) AS email_id
  FROM club_member_info_cleaned
```
#Kết quả 10 dòng đầu
```
| full_name        | age | marital_status | email                          | phone        | full_address                              | job_title                   | membership_date | state      | street              | city       | email_id |
|------------------|-----|----------------|--------------------------------|--------------|-------------------------------------------|-----------------------------|-----------------|------------|---------------------|------------|----|
| ARDA ALLAM       | 36  | married        | aallam64@nps.gov               | 415-797-9281 | 86 Sunfield Parkway,San Rafael,California | Human Resources Assistant I | 1/10/2012       | California | 86 Sunfield Parkway | San Rafael | 1  |
| ARDA ALLAM       | 36  | married        | aallam64@nps.gov               | 415-797-9281 | 86 Sunfield Parkway,San Rafael,California | Human Resources Assistant I | 1/10/2012       | California | 86 Sunfield Parkway | San Rafael | 2  |
| ARDA ALLAM       | 36  | married        | aallam64@nps.gov               | 415-797-9281 | 86 Sunfield Parkway,San Rafael,California | Human Resources Assistant I | 1/10/2012       | California | 86 Sunfield Parkway | San Rafael | 3  |
| AERIELL ANGELINI | 32  | married        | aangelinilu@whitehouse.gov     | 806-508-7374 | 0 Brown Street,Amarillo,Texas             | Database Administrator I    | 7/10/2013       | Texas      | 0 Brown Street      | Amarillo   | 1  |
| AERIELL ANGELINI | 32  | married        | aangelinilu@whitehouse.gov     | 806-508-7374 | 0 Brown Street,Amarillo,Texas             | Database Administrator I    | 7/10/2013       | Texas      | 0 Brown Street      | Amarillo   | 2  |
| AERIELL ANGELINI | 32  | married        | aangelinilu@whitehouse.gov     | 806-508-7374 | 0 Brown Street,Amarillo,Texas             | Database Administrator I    | 7/10/2013       | Texas      | 0 Brown Street      | Amarillo   | 3  |
| AURORE AVERILL   | 28  | married        | aaverill5i@theglobeandmail.com | 713-330-3502 | 42107 Debs Court,Houston,Texas            | Account Coordinator         | 5/11/2019       | Texas      | 42107 Debs Court    | Houston    | 1  |
| AURORE AVERILL   | 28  | married        | aaverill5i@theglobeandmail.com | 713-330-3502 | 42107 Debs Court,Houston,Texas            | Account Coordinator         | 5/11/2019       | Texas      | 42107 Debs Court    | Houston    | 2  |
| AURORE AVERILL   | 28  | married        | aaverill5i@theglobeandmail.com | 713-330-3502 | 42107 Debs Court,Houston,Texas            | Account Coordinator         | 5/11/2019       | Texas      | 42107 Debs Court    | Houston    | 3  |
| ANNEMARIE BALSOM | 52  | divorced       | abalsomny@wired.com            | 718-247-6744 | 723 Caliangt Park,Jamaica,New York        | Design Engineer             | 3/6/2014        | New York   | 723 Caliangt Park   | Jamaica    | 1  |
```
#Xóa các dòng có email_id > 1
```
WITH Ranked AS (
  SELECT ROWID AS id,
         ROW_NUMBER() OVER (PARTITION BY email ORDER BY DATE(membership_date) DESC) AS email_id
  FROM club_member_info_cleaned
)
DELETE FROM club_member_info_cleaned
WHERE id IN (
  SELECT id FROM Ranked WHERE email_id > 1
);
```
