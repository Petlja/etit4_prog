# Класе Data Provider-a

У програмском језику *C#*, односно у *.NET Framework-у*, када се ради са базама
података, за комуникацију са различитим типовима база података користе се
*ADO.NET* добављачи података, односно *ADO.NET Data Provider*-и. Сваки
*Data Provider* обухвата одређени скуп класа које омогућавају рад са базом, као
што су:

* **Connection** користи се за конекцију са базом,
* **Command** се користи за извршавање SQL упита или процедура,
* **DataReader** омогућава читање података из базе ред по ред,
* **DataAdapter** омогућава пуњење *DataSet*-а и рад без сталне конекције и
* **Parameter** се користи за параметризоване упите или процедуре.

*DataSet* представља репрезентација података у меморији која омогућава рад без
директне везе са базом након учитавања података. Више о томе учићеш у наредним
лекцијама.

Најчешћи *.NET Framework Data Provider*-и су:

| Provider     | Connection класа | Command класа    | Именски простор          |
|--------------|------------------|------------------|--------------------------|
| SQL Server   | SqlConnection    | SqlCommand       | System.Data.SqlClient    |
| Entity       | EntityCommand    | EntityConnection | System.Data.EntityClient |
| OLE DB       | OleDbConnection  | OleDbCommand     | System.Data.OleDb        |
| ODBC         | OdbcConnection   | OdbcCommand      | System.Data.Odbc         |
| Oracle       | OracleConnection | OracleCommand    | System.Data.OracleClient |

## SQL Server Data Provider

*SQL Server Data Provider* је један од најважнијих ADO.NET провајдера и део је
*.NET Framework*-а. Дефинисан је у именском простору
[System.Data.SqlClient](https://learn.microsoft.com/en-us/dotnet/api/system.data.sqlclient?view=netframework-4.8).
Специјално је оптимизован за рад са *Microsoft SQL Server*-ом и пружа директан,
високо ефикасан начин за рад са *SQL Server* базама података. Потпуно је
интегрисан са Visual Studio алатима попут дизајнера, *IntelliSense*-а и др.
Безбедна обрада података омогућена је параметризованим упитима или процедурама,
чиме се спречава *SQL Injection* и повећава безбедност апликације.
*SQL Server Data Provider* подржава и connection pooling, што омогућава поновну
употребу отворених конекција и унапређује перформансе апликације.

Најважније класе у *SQL Server Data Provider*-у су:

* `SqlConnection`, користи се за конекцију са базом,
* `SqlCommand`, користи се за извршавање SQL упита или покретање складиштених процедура,
* `SqlDataReader`, омогућава читање података из базе ред по ред,
* `SqlDataAdapter`, омогућава пуњење *DataSet*-а и рад без сталне конекције,
* `SqlParameter`, се користи за параметризоване упите или процедуре и
* `SqlTransaction`, омогућава контролу над трансакцијама.

*SQL Server Data Provider* је једини *Data Provider* којег ћеш користити у овом
поглављу.
