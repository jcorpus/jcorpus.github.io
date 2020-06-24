---
layout: post
title: "Conectando Prolog con Sql Server"
featured-img: emile-perron-190221
summary: Markdown is a way to style text on the web. You control the display of the document; formating words as bold 
categories: [Prolog]
---
<h2 class="section-heading">Conectando Prolog con Sql Server</h2>
<p>En este post realizaremos un ejercicio de Sistemas expertos basados en probabilidad, la probabilidad Bayesiana</p>
<p style="text-align:justify;">Conectaremos Prolog con SQLServer en la plataforma windows. Empezaremos creando la base de datos en sql server, con el nombre "hospital" y con una tabla llamada resumen </p>
<h4>Creamos la tabla resumen con los siguientes campos.</h4>
<img src="{{ site.baseurl }}/assets/img/posts/prolog-sql/tabla.png" alt="tabla prolog">
<br>
<h4>Llenamos la tabla con los siguientes datos.</h4>
```sql
insert into resumen (dolor,ppeso,vomito,enfermedad,cantidad)values('si','si','si','si',220);
insert into resumen (dolor,ppeso,vomito,enfermedad,cantidad)values('si','si','no','si',220);
insert into resumen (dolor,ppeso,vomito,enfermedad,cantidad)values('si','no','si','si',25);
insert into resumen (dolor,ppeso,vomito,enfermedad,cantidad)values('si','no','no','si',25);
insert into resumen (dolor,ppeso,vomito,enfermedad,cantidad)values('no','si','si','si',95);
insert into resumen (dolor,ppeso,vomito,enfermedad,cantidad)values('no','si','no','si',95);
insert into resumen (dolor,ppeso,vomito,enfermedad,cantidad)values('no','no','si','si',10);
insert into resumen (dolor,ppeso,vomito,enfermedad,cantidad)values('no','no','no','si',10);

insert into resumen (dolor,ppeso,vomito,enfermedad,cantidad)values('si','si','si','no',4);
insert into resumen (dolor,ppeso,vomito,enfermedad,cantidad)values('si','si','no','no',9);
insert into resumen (dolor,ppeso,vomito,enfermedad,cantidad)values('si','no','si','no',5);
insert into resumen (dolor,ppeso,vomito,enfermedad,cantidad)values('si','no','no','no',12);
insert into resumen (dolor,ppeso,vomito,enfermedad,cantidad)values('no','si','si','no',31);
insert into resumen (dolor,ppeso,vomito,enfermedad,cantidad)values('no','si','no','no',76);
insert into resumen (dolor,ppeso,vomito,enfermedad,cantidad)values('no','no','si','no',50);
insert into resumen (dolor,ppeso,vomito,enfermedad,cantidad)values('no','no','no','no',113);
```
