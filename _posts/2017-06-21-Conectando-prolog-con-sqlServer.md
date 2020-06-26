---
layout: post
title: "Conectando Prolog con Sql Server"
featured-img: prolog-sql/prolog/imagen-prolog
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
<img src="{{ site.baseurl }}/assets/img/posts/prolog-sql/resumen.PNG" alt="datos de la tabla">
<br>
<h2>Ahora crearemos un nuevo origen de Datos llamado "NuevaConexion".</h2>
<h4>Nos dirigimos a herramientas administrativas, luego a origen de datos ODBC</h4>
 <img src="{{ site.baseurl }}/assets/img/posts/prolog-sql/herramientas-administrativas.png" alt="Post Sample Image">
 <h4>Agregamos uno nuevo</h4>
  <img src="{{ site.baseurl }}/assets/img/posts/prolog-sql/nuevo-origen-de-datos.png" alt="Post Sample Image">
  <h4>Seleccionamos SQL native client</h4>
  <img src="{{ site.baseurl }}/assets/img/posts/prolog-sql/seleccionamos-sql-native.png" alt="Post Sample Image">
 <h4>Colocamos el nombre a nuestra conexion y en servidor colocamos un punto "."</h4>
 <img src="{{ site.baseurl }}/assets/img/posts/prolog-sql/colocamos-el-nombre-a-nuestra-conexion-y-un-servidor.png" alt="Post Sample Image">
 <h4>Seleccionamos autentificacion de Windows</h4>
 <img src="{{ site.baseurl }}/assets/img/posts/prolog-sql/autentificacion-de-windows.png" alt="Post Sample Image">
 <h4>Seleccionamos la base de datos creada</h4>
 <img src="{{ site.baseurl }}/assets/img/posts/prolog-sql/seleccionamos-la-base-de-datos-creada.png" alt="Post Sample Image">
 <h4>Establecemos el idioma</h4>
 <img src="{{ site.baseurl }}/assets/img/posts/prolog-sql/establecemos-el-idioma.png" alt="Post Sample Image"> 
  <h4>Probamos el origen de datos y verificamos que la prueba se completo correctamente y aceptamos.</h4>
 <img src="{{ site.baseurl }}/assets/img/posts/prolog-sql/probamos-el-origen-de-datos.png" alt="Post Sample Image">
 <br>
 <h4>Conexión establecida.</h4>
 <img src="{{ site.baseurl }}/assets/img/posts/prolog-sql/conexion-establecida.png" alt="Post Sample Image">
 
 <h2 class="section-heading">Ahora comenzaremos con Prolog</h2>
 <p>Para este tutorial trabajaré con SwiProlog, la instalación es sencilla, puedes descargarlo desde <a href="http://www.swi-prolog.org/Download.html"  target="_blank">Aquí</a></p>
 <p>Comenzaremos escribiendo el siguiente codigo prolog, y el nombre de la conexión que creamos para este tutorial "NuevaConexion", en usuario lo dejamos en root y password en blanco en mi caso.</p>
 <p>Seleccionamos la cantidad de la tabla resumen.</p>
 ```prolog
 conexion:- odbc_connect('NuevaConexion',_,[user('root'),password(''),alias(bd),open(once)]),
           odbc_prepare(bd,'SELECT cantidad FROM resumen where enfermedad =? and dolor=? and vomito=? and ppeso=?',
           [atom>char(2),atom>char(2),atom>char(2),atom>char(2)],
           Handle,
           [types([integer])]),
           abolish(odbc_handle/1),
           assert(odbc_handle(Handle)).



run_stmt(R,E,D,V,Pp):- odbc_handle(Handle),
              odbc_execute(Handle,[E,D,V,Pp],row(R)).


sistema:- conexion,
           writeln('Dolor:'),
           read(D),
           writeln('Vomito:'),
           read(V),
           writeln('Perdida de peso:'),
           read(Pp),
           run_stmt(R1,si,D,V,Pp),
           run_stmt(R2,no,D,V,Pp),
           Bayes is R1/(R1+R2),
           format('La probabilidad bayesiana de tener la enfermedad es ~w',[Bayes]).


 
 ```
 <p>Compilamos y ejecutamos el predicado sistema.</p>
 <img src="{{ site.baseurl }}/assets/img/posts/prolog-sql/resultado1.png" alt="Post Sample Image">
 
 
 <h4>Ahora realizarenos el mismo ejercicio usando el modo gráfico de Prolog.</h4>
 ```prolog
 conexion:- odbc_connect('NuevaConexion',_,[user('root'),password(''),alias(bd),open(once)]),
           odbc_prepare(bd,'SELECT cantidad from resumen where enfermedad=? and dolor=? and vomito=? and ppeso=?',
           [atom>char(2),atom>char(2),atom>char(2),atom>char(2)],
           Handle,
           [types([integer])]),
           abolish(odbc_handle/1),
           assert(odbc_handle(Handle)).



run_stmt(R,E,D,V,Pp):- odbc_handle(Handle),
              odbc_execute(Handle,[E,D,V,Pp],row(R)).


sistema:-  conexion,
	   new(V, dialog('Sistema experto probabilistico')),
	   new(P1,menu('dolor?')),
	   send_list(P1,append, [si,no]),
	   new(P2,menu('perdida de peso?')),
	   send_list(P2,append, [si,no]),
	   new(P3,menu('vomito?')),
	   send_list(P3,append, [si,no]),
	   send_list(V,append,[
		       P1,P2,P3,
		       button(aceptar,message(@prolog,
					     inferencia,
					     P1?selection,
					     P2?selection,
					     P3?selection))]),
	   send(V,open).


inferencia(D,Pp,V):- conexion,
	     run_stmt(R1,si,D,V,Pp),
             run_stmt(R2,no,D,V,Pp),
             Bayes is R1/(R1+R2),
	     new(W,dialog('Teorema de Bayes')),
	     new(E,label(l,Bayes)),
	     send(W,append,E),
	     send(W,open).

 
 
 ```
<p>Compilamos y llamamos al predicado sistema, seleccionamos los sintomas y la probabilidad bayesiana de tener la enfermedad, con el siguiente resultado.</p>
<img src="{{ site.baseurl }}/assets/img/posts/prolog-sql/resultado2.png" alt="Post Sample Image">
<p>El codigo fuente de prolog lo puedes descargar desde <a href="https://github.com/jcorpus/USP/tree/master/USP9/Inteligencia_artificial/prolog">Aquí</a><p>
