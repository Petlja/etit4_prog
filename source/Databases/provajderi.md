# Класе Data Provider-a

У програмском језику *C#*, односно у *.NET Framework-у*, када се ради са базама
података, за комуникацију са различитим типовима база података користе се
*ADO.NET* добављачи података, односно *ADO.NET Data Provider*-и. Сваки
*Data Provider* обухвата одређени скуп класа које омогућавају рад са базом, као
што су:

* **Connection** – за повезивање са базом података
* **Command** – за слање SQL упита или позив процедура
* **DataReader** – за читање података из базе конекционом режиму
* **DataAdapter** – за попуњавање *DataSet*-а и рад у бесконекционом режиму
* **Parameter** – за прослеђивање параметара у упитима

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
Најважније класе у *SQL Server Data Provider*-у су:
