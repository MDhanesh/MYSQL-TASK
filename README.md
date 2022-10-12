# MYSQL-TASK1
CREATE DATABASE IMDB;
-- 1)Movie should have multiple media
CREATE TABLE Movie (
    Movie_id INT PRIMARY KEY AUTO_INCREMENT,
    Movie_name VARCHAR(255) NOT NULL,
    Director VARCHAR(255),
    Movie_length VARCHAR(255),
    Release_date VARCHAR(255),
    Language VARCHAR(255)
);
INSERT INTO Movie(Movie_name,Director,Movie_length,Release_date,Language) VALUES("PS-1","ManiRatnam","145_min",30-09-2022,"Tamil"),("KGF-1","PrashanthNeel","135_min",20-12-2018,"Kannada");
SELECT * FROM Movie;
CREATE TABLE Media (
    Media_id INT PRIMARY KEY AUTO_INCREMENT,
    Media VARCHAR(255),
    Type VARCHAR(255),
    URL VARCHAR(255),
    Movie_id INT,
    FOREIGN KEY (Movie_id)
        REFERENCES Movie (Movie_id)
);
INSERT INTO Media(Media,Type,URL,Movie_id) VALUES ("Image1","img.jpg","https://images.news18.com/ibnlive/uploads/2022/09/ps1.jpg",1),("Vedio","vs.mp4","https://www.youtube.com/watch?v=Oh5sU8YzF1A",1),("Vedio","Vs.mp4","https://www.youtube.com/watch?v=3oxo-wacE50",2);
SELECT * FROM Media;
SELECT * FROM movie  INNER JOIN media ON movie.Movie_id=media.Movie_id;

-- 2)Movie can belongs to multiple Genre
CREATE TABLE Genre (
    GenreID INT PRIMARY KEY AUTO_INCREMENT,
    Genre VARCHAR(255)
);
INSERT INTO Genre(Genre) VALUES ("Adventure"),("Thriller"),("Action"),("Love");
SELECT * FROM Genre;
CREATE TABLE Movie_Genre (
    ID INT PRIMARY KEY AUTO_INCREMENT,
    GenreID INT,
    FOREIGN KEY (GenreID)
        REFERENCES Genre (GenreID),
    Movie_id INT,
    FOREIGN KEY (Movie_id)
        REFERENCES Movie (Movie_id)
);
INSERT INTO Movie_Genre(GenreID,Movie_id) VALUES (1,1),(2,1),(3,1),(2,2),(3,2),(4,2);
SELECT * FROM Movie_Genre;
SELECT * FROM movie_genre INNER JOIN movie ON movie.Movie_id=movie_genre.Movie_id JOIN Genre on movie_genre.GenreID=Genre.GenreID;

-- 3) Movie can have multiple reviews and Review can belongs to a user
CREATE TABLE Users (
    UsersID INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255),
    Age INT
);
INSERT INTO Users (Name,Age) VALUES ("XXX",24),("YYY",22);
CREATE TABLE Reviews (
    ID INT PRIMARY KEY AUTO_INCREMENT,
    Movie_id INT,
    FOREIGN KEY (Movie_id)
        REFERENCES Movie (Movie_id),
    UsersID INT,
    FOREIGN KEY (UsersID)
        REFERENCES Users (UsersID),
    Ratings FLOAT
);
INSERT INTO Reviews (Movie_id,UsersID,Ratings) VALUES(1,1,4.8),(1,2,4.7),(2,1,4.5),(2,2,4.6);
SELECT * FROM Users;
SELECT * FROM Reviews;
SELECT * FROM movie INNER JOIN reviews ON movie.Movie_id=reviews.usersID;

-- 4)Artist can have multiple skills
CREATE TABLE Artist (
    ArtistID INT PRIMARY KEY AUTO_INCREMENT,
    Artist_Name VARCHAR(255),
    Age INT,
    Experience INT
);
INSERT INTO Artist(Artist_name,Age,Experience) VALUES ("Vikram",56,22),("YASH",40,15),("JayamRavi",42,20);
SELECT * FROM Artist;

CREATE TABLE Skills (
    SkillsID INT PRIMARY KEY AUTO_INCREMENT,
    Skills VARCHAR(255)
);
INSERT INTO Skills(Skills) VALUES ("Dancer"),("Singer"),("Writing"),("Acting");
SELECT * FROM Skills;

CREATE TABLE ArtistSkill (
    ID INT PRIMARY KEY AUTO_INCREMENT,
    ArtistID INT,
    FOREIGN KEY (ArtistID)
        REFERENCES Artist (ArtistID),
    SkillsID INT,
    FOREIGN KEY (SkillsID)
        REFERENCES Skills (SkillsID)
);
INSERT INTO ArtistSkill(ArtistID,SkillsID) VALUES (1,1),(1,2),(1,4),(2,1),(2,3),(2,4),(3,2),(3,4);
SELECT * FROM ArtistSkill;

SELECT  * FROM Artistskill  JOIN skills ON artistskill.skillsID=skills.skillsID JOIN artist ON artistskill.ArtistID=artist.ArtistID ;
SELECT Artist_Name,Skills FROM Artistskill JOIN artist ON artist.ArtistID=artistskill.ArtistID JOIN skills ON artistskill.SkillsID=skills.SkillsID;

-- LINKING Artist and the ACTED MOVIES
CREATE TABLE Acted_Movies (
    ID INT PRIMARY KEY AUTO_INCREMENT,
    ArtistID INT,
    FOREIGN KEY (ArtistID)
        REFERENCES Artist (ArtistID),
    moviesID INT,
    FOREIGN KEY (MoviesID)
        REFERENCES Movie (Movie_id)
);
INSERT INTO Acted_movies(ArtistID,MoviesID) VALUES (1,1),(3,1),(2,2);
SELECT * FROM Acted_Movies;
SELECT Movie_name,Artist_Name,Director,Language,Release_date FROM acted_movies JOIN movie ON acted_movies.moviesID=movie.Movie_id JOIN artist ON acted_movies.ArtistID=artist.ArtistID ;

-- 5) Artist can perform multiple role in a single film

CREATE TABLE Roles (
    RolesID INT PRIMARY KEY AUTO_INCREMENT,
    ActingRole VARCHAR(255)
);
INSERT INTO Roles(ActingRole) VALUES ("Hero"),("Villan"),("Friend");
SELECT * FROM Roles;

CREATE TABLE MovieRole (
    ID INT PRIMARY KEY AUTO_INCREMENT,
    ArtistID INT,
    FOREIGN KEY (ArtistID)
        REFERENCES Artist (ArtistID),
    moviesID INT,
    FOREIGN KEY (MoviesID)
        REFERENCES Movie (Movie_id),
    RolesID INT,
    FOREIGN KEY (RolesID)
        REFERENCES Roles (RolesID)
);
INSERT INTO MovieRole(ArtistID,MoviesID,RolesID) VAlUES (1,1,1),(3,1,3),(2,2,1),(2,2,2);
SELECT * FROM MovieRole;
SELECT Movie_name,ActingRole,Artist_Name FROM movierole JOIN roles ON movierole.RolesID=roles.RolesID JOIN artist ON movierole.ArtistID=artist.ArtistID Join movie ON movierole.moviesID=movie.Movie_id;


-- END OF IMDB TABLE DESIGN
