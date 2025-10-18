# Lab_ConsultasDB


drop schema if exists school;
create schema school;
use school;

-- Create table students
create table students(
id int auto_increment primary key,
first_name varchar (50),
last_name varchar (50),
age int,
class varchar (50),
grade decimal(4,2), 
email varchar (150)
);

-- insert data
insert into students (first_name, last_name, age, class, grade, email) values
('Anna', 'Smith', 20, 'Class A', 8.5, 'anna@gmail.com'),
('John', 'Doe', 22, 'Class B', 6.7, NULL),
('Maria', 'Johnson', 21, 'Class A', 9.1, 'maria@yahoo.com'),
('Peter', 'Brown', 20, 'Class B', 5.4, 'peter@hotmail.com'),
('Lucy', 'White', 19, 'Class A', 7.0, NULL),
('Michael', 'Taylor', 23, 'Class C', 4.9, 'michael@gmail.com'),
('Sarah', 'Davis', 22, 'Class C', 8.8, 'sarah@hotmail.com'),
('George', 'Wilson', 20, 'Class B', 6.2, 'george@gmail.com'),
('Emma', 'Smith', 21, 'Class A', 7.5, 'emma.smith@gmail.com'), -- Same last name as Anna
('David', 'Smith', 22, 'Class B', 6.8, 'david.smith@yahoo.com'); -- Another "Smith"

select class, count(*) q_students, AVG(grade)avg_grade
from students s 
group by class 
order by avg_grade asc;

select * from students s where s.last_name like '%r';

select * from students s where s.email is not null;

# create database if not exists podcast_dib;
# use podcast_dib;

create table usuarios(
id int auto_increment primary key,
nombre varchar(100) not null,
email varchar(150) unique not null
);

create table podcast(
id int auto_increment primary key,
titulo varchar(150) not null,
description text,
autor varchar(100),
fecha_publicacion date 
);

create table descargas(
id int auto_increment primary key,
id_usuario int not null,
id_podcast int not null,
fecha_descarga DATETIME default CURRENT_TIMESTAMP(),
foreign key (id_usuario) references usuarios(id),
foreign key (id_podcast) references podcast(id)
);

insert into usuarios(nombre, email) values
("José Fernadez", "j.fernandez@email.com"),
('Pedro Martínez',"p.martinez@email.com"),
('Luisa Díaz', "l.diaz@email.com");

insert into podcast (titulo,description, autor, fecha_publicacion) values
("El futuro de la IA", "Acerca de la IA", "David Sanchez", "2024-05-12"),
("Viajes y aventuras", "Podcast de viajes por el mundo", "Lucía Gómez","2024-06-01"),
("Historias de código", "Experiencias de desarrolladores", "Carlos Pérez","2024-06-15");

insert into descargas (id_usuario, id_podcast, fecha_descarga) values
(1,1,NOW()),
(2,1,NOW()),
(2,2,NOW()),
(1,3,NOW());

select*from descargas d
join usuarios u on d.id_usuario =u.id 
join podcast p on d.id_podcast =p.id
where u.nombre ="Pedro Martínez";

select d.fecha_descarga , u.nombre , p.titulo from descargas d
join usuarios u on d.id_usuario =u.id 
join podcast p on d.id_podcast =p.id
where u.nombre ="Pedro Martínez";

select p.titulo, COUNT(*) as total_descargas
from descargas d 
join podcast p on d.id =p.id 
group by p.titulo;

create table detalles_pedido(
id_podcast int primary key,
peso_archivo int,
duracion_ep time ,
enlace_descarga varchar(100),
formato varchar(30),
estado varchar (50),
foreign key (id_podcast) references podcast (id)
);

insert into detalles_pedido(id_podcast,peso_archivo,duracion_ep,enlace_descarga,formato,estado) values 
(1,590,'1:12:20','https://descargas.com/ia.mp3','mp3','Disponible'),
(2,590,'0:12:20','https://descargas.com/viajes.mp3','mp3','Mantenimiento'),
(3,590,'1:12:20','https://descargas.com/experiencia.mp3','mp3','Disponible');

select* from podcast p 
join detalles_pedido dp on p.id =dp.id_podcast;

select sum(dp.peso_archivo) as peso_total_MB
from detalles_pedido dp 
where dp.estado= 'disponible';

