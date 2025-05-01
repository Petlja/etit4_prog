# Мапирање SQL Server типова података

| SQL Server тип                            | C# тип           | .NET тип                | SqlDbType enum     | SqlTypes приступ          | DbType enum             | DbType приступ      |
|-------------------------------------------|------------------|-------------------------|--------------------|---------------------------|-------------------------|---------------------|
| `bigint`                                  | `long`           | `System.Int64`          | `BigInt`           | `GetSqlInt64`             | `Int64`                 | `GetInt64`          |
| `binary`                                  | `byte[]`         | `System.Byte[]`         | `Binary`           | `GetSqlBinary`            | `Binary`                | `GetBytes`          |
| `bit`                                     | `bool`           | `System.Boolean`        | `Bit`              | `GetSqlBoolean`           | `Boolean`               | `GetBoolean`        |
| `char`                                    | `string`         | `System.String`         | `Char`             | `GetSqlString`            | `AnsiStringFixedLength` | `GetString`         |
| `date`                                    | `DateTime`       | `System.DateTime`       | `Date`             | `GetSqlDateTime`          | `Date`                  | `GetDateTime`       |
| `datetime`                                | `DateTime`       | `System.DateTime`       | `DateTime`         | `GetSqlDateTime`          | `DateTime`              | `GetDateTime`       |
| `datetime2`                               | `DateTime`       | `System.DateTime`       | `DateTime2`        | `-`                       | `DateTime2`             | `GetDateTime`       |
| `datetimeoffset`                          | `DateTimeOffset` | `System.DateTimeOffset` | `DateTimeOffset`   | `-`                       | `DateTimeOffset`        | `GetDateTimeOffset` |
| `decimal`                                 | `decimal`        | `System.Decimal`        | `Decimal`          | `GetSqlDecimal`           | `Decimal`               | `GetDecimal`        |
| `FILESTREAM attribute (varbinary(max))`   | `byte[]`         | `System.Byte[]`         | `VarBinary`        | `GetSqlBytes`             | `Binary`                | `GetBytes`          |
| `float`                                   | `double`         | `System.Double`         | `Float`            | `GetSqlDouble`            | `Double`                | `GetDouble`         |
| `image`                                   | `byte[]`         | `System.Byte[]`         | `Binary`           | `GetSqlBinary`            | `Binary`                | `GetBytes`          |
| `int`                                     | `int`            | `System.Int32`          | `Int`              | `GetSqlInt32`             | `Int32`                 | `GetInt32`          |
| `money`                                   | `decimal`        | `System.Decimal`        | `Money`            | `GetSqlMoney`             | `Decimal`               | `GetDecimal`        |
| `nchar`                                   | `string`         | `System.String`         | `NChar`            | `GetSqlString`            | `StringFixedLength`     | `GetString`         |
| `ntext`                                   | `string`         | `System.String`         | `NText`            | `GetSqlString`            | `String`                | `GetString`         |
| `numeric`                                 | `decimal`        | `System.Decimal`        | `Decimal`          | `GetSqlDecimal`           | `Decimal`               | `GetDecimal`        |
| `nvarchar`                                | `string`         | `System.String`         | `NVarChar`         | `GetSqlString`            | `String`                | `GetString`         |
| `real`                                    | `float`          | `System.Single`         | `Real`             | `GetSqlSingle`            | `Single`                | `GetFloat`          |
| `rowversion`                              | `byte[]`         | `System.Byte[]`         | `Timestamp`        | `GetSqlBinary`            | `Binary`                | `GetBytes`          |
| `smalldatetime`                           | `DateTime`       | `System.DateTime`       | `SmallDateTime`    | `GetSqlDateTime`          | `DateTime`              | `GetDateTime`       |
| `smallint`                                | `short`          | `System.Int16`          | `SmallInt`         | `GetSqlInt16`             | `Int16`                 | `GetInt16`          |
| `smallmoney`                              | `decimal`        | `System.Decimal`        | `SmallMoney`       | `GetSqlMoney`             | `Decimal`               | `GetDecimal`        |
| `sql_variant`                             | `object`         | `System.Object`         | `Variant`          | `GetSqlValue`             | `Object`                | `GetValue`          |
| `text`                                    | `string`         | `System.String`         | `Text`             | `GetSqlString`            | `String`                | `GetString`         |
| `time`                                    | `TimeSpan`       | `System.TimeSpan`       | `Time`             | `-`                       | `Time`                  | `GetTimeSpan`       |
| `timestamp`                               | `byte[]`         | `System.Byte[]`         | `Timestamp`        | `GetSqlBinary`            | `Binary`                | `GetBytes`          |
| `tinyint`                                 | `byte`           | `System.Byte`           | `TinyInt`          | `GetSqlByte`              | `Byte`                  | `GetByte`           |
| `uniqueidentifier`                        | `Guid`           | `System.Guid`           | `UniqueIdentifier` | `GetSqlGuid`              | `Guid`                  | `GetGuid`           |
| `varbinary`                               | `byte[]`         | `System.Byte[]`         | `VarBinary`        | `GetSqlBinary`            | `Binary`                | `GetBytes`          |
| `varchar`                                 | `string`         | `System.String`         | `VarChar`          | `GetSqlString`            | `AnsiString`            | `GetString`         |
| `xml`                                     | `string`         | `System.String`         | `Xml`              | `GetSqlXml`               | `Xml`                   | `GetValue`          |

Напомена: Иако су синоними у *SQL Server*-у, треба напоменути да се `timestamp`
сматра застарелим, а `rowversion` препорученим типом.
