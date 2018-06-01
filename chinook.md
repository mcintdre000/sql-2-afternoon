**Practice joins**

SELECT *
FROM Invoice i
JOIN InvoiceLine il ON il.invoiceId = i.invoiceId
WHERE il.UnitPrice > 0.99;

SELECT i.InvoiceDate, c.FirstName, c.LastName, i.Total
FROM Invoice i
JOIN Customer c ON i.CustomerId = c.CustomerId;

SELECT c.FirstName, c.LastName, e.FirstName, e.LastName
FROM Customer c
JOIN Employee e ON e.EmployeeID = c.SupportRepId;

SELECT al.Title, ar.Name
FROM Album al
JOIN Artist ar ON al.ArtistId = ar.ArtistId;

SELECT pt.trackId
FROM Playlist p
JOIN PlaylistTrack pt ON p.PlaylistId = pt.PlaylistId
WHERE  p.Name = "Music";

SELECT t.Name
FROM Track t
JOIN PlaylistTrack pt ON t.TrackId = pt.TrackId
WHERE  pt.PlaylistId = 5;

SELECT t.Name, p.Name
FROM Playlist p
JOIN PlaylistTrack pt ON p.PlaylistId = pt.PlaylistId
JOIN Track t ON t.TrackId = pt.TrackId;

SELECT t.Name, al.Title
FROM Track t
JOIN Album al ON t.AlbumId = al.AlbumId
JOIN Genre g ON t.GenreId = g.GenreId
WHERE g.Name = "Alternative";

**Black Diamond**

SELECT DISTINCT t.TrackId, t.Name, g.Name, al.Title, ar.Name
FROM Track t
JOIN Album al ON t.AlbumId = al.AlbumId
JOIN Genre g ON t.GenreId = g.GenreId
JOIN PlaylistTrack pt ON t.TrackId = pt.TrackId
JOIN Playlist p ON pt.TrackId = t.Trackid
JOIN Artist ar ON al.ArtistId = ar.ArtistId
WHERE p.Name = "Music";

**Practice nested queries**

SELECT * 
FROM Invoice 
WHERE InvoiceId IN ( SELECT InvoiceId FROM InvoiceLine WHERE UnitPrice > 0.99 );

SELECT *
FROM PlaylistTrack
WHERE PlaylistId IN ( SELECT PlaylistId FROM Playlist WHERE Name = "Music");

SELECT Name
FROM Track
WHERE TrackId IN ( SELECT TrackId FROM PlaylistTrack WHERE PlayListId = 5);

SELECT *
FROM Track
WHERE GenreId IN ( SELECT GenreId FROM Genre WHERE Name = "Comedy");

SELECT *
FROM Track
WHERE AlbumId IN ( SELECT AlbumId FROM Album WHERE Title = "Fireball");

SELECT *
FROM Track
WHERE AlbumId IN ( SELECT AlbumId FROM Album WHERE ArtistId IN ( SELECT ArtistId FROM Artist WHERE Name = "Queen"));

**Practice updating Rows**

UPDATE Customer
SET Fax = null
WHERE Fax IS NOT null;

UPDATE Customer
SET Company = "Self"
WHERE Company IS null;

UPDATE Customer
SET LastName = "Thompson"
WHERE FirstName IS "Julia" AND LastName IS "Barnett";

UPDATE Customer
SET SupportRepId = 4
WHERE Email IS "luisrojas@yahoo.cl";

UPDATE Track
SET Composer = "The darkness around us"
WHERE GenreId = ( SELECT GenreId FROM Genre WHERE Name = "Metal" )
AND Composer is null;

**Group by**

SELECT Count(*), g.Name
FROM Track t
JOIN Genre g ON t.GenreId = g.GenreId
GROUP BY g.Name;

SELECT Count(*), g.Name
FROM Track t
JOIN Genre g ON g.GenreId = t.GenreId
WHERE g.Name = 'Pop' OR g.Name = 'Rock'
GROUP BY g.Name;

SELECT ar.Name, Count(*)
FROM Artist ar
JOIN Album al ON ar.ArtistId = al.ArtistId
GROUP BY al.ArtistId;

**Use Distinct**

SELECT DISTINCT Composer
FROM Track;

SELECT DISTINCT BillingPostalCode
FROM Invoice;

SELECT DISTINCT Company
FROM Customer;

**Delete Rows**

CREATE TABLE practice_delete ( Name string, Type string, Value integer );
INSERT INTO practice_delete ( Name, Type, Value ) VALUES ("delete", "bronze", 50);
INSERT INTO practice_delete ( Name, Type, Value ) VALUES ("delete", "bronze", 50);
INSERT INTO practice_delete ( Name, Type, Value ) VALUES ("delete", "bronze", 50);
INSERT INTO practice_delete ( Name, Type, Value ) VALUES ("delete", "silver", 100);
INSERT INTO practice_delete ( Name, Type, Value ) VALUES ("delete", "silver", 100);
INSERT INTO practice_delete ( Name, Type, Value ) VALUES ("delete", "gold", 150);
INSERT INTO practice_delete ( Name, Type, Value ) VALUES ("delete", "gold", 150);
INSERT INTO practice_delete ( Name, Type, Value ) VALUES ("delete", "gold", 150);
INSERT INTO practice_delete ( Name, Type, Value ) VALUES ("delete", "gold", 150);

SELECT * FROM practice_delete;

DELETE 
FROM practice_delete 
WHERE Type = "bronze";

SELECT * FROM practice_delete;

DELETE 
FROM practice_delete 
WHERE Type = "silver";

SELECT * FROM practice_delete;

DELETE 
FROM practice_delete 
WHERE Value = 150;

SELECT * FROM practice_delete;

**eCommerce Simulation - No Hints**

CREATE TABLE users ( User_Id SERIAL PRIMARY KEY, Name TEXT, Email TEXT );
INSERT INTO users  ( Name, Email ) 
VALUES ( "Drew", "drew@email.co" ),
	   ( "Adam", "adam@email.co" ),
       ( "Brandon", "brando@email.co" );

CREATE TABLE products ( Products_Id SERIAL PRIMARY KEY, Name TEXT, Price DECIMAL );
INSERT INTO products ( Name, Price ) 
VALUES ( "Shirt", 15.00 ),
	   ( "Pants", 49.99 ),
       ( "Shoes", 69.99 );
       
CREATE TABLE orders ( Order_id SERIAL PRIMARY KEY, Products_Id INT, User_Id INT );
INSERT INTO orders ( Products_Id, User_Id )
VALUES ( 1, 1 ),
	   ( 2, 3 ),
       ( 1, 2 );
       
SELECT * FROM users;