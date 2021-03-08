```
스프링에서 JDBC의 Properties 설정하는 방법에 대해서 알아보겠습니다.대표적으로 많이 사용하는 DB인 MySQL, Oracle, MS-SQL의 연결방법입니다.
- MySQLJDBC.Driver=com.mysql.jdbc.DriverJDBC.ConnectionURL=jdbc:mysql://URL주소:포트번호/DB명JDBC.Username=계정명JDBC.Password=비밀번호

[출] JDBC별 properties 정보|작성자 창천향로
EX) JDBC.Driver=com.mysql.jdbc.DriverJDBC.ConnectionURL=jdbc:mysql://localhost:12345/CrazyKimJDBC.Username=CrazyKimJDBC.Password=CrazyKim
[출처] JDBC별 properties 정ㅇ보|작성자 창천향로

- OracleJDBC.DriverClassName=oracle.jdbc.driver.OracleDriverJDBC.url=jdbc:oracle:thin@URL주소:1521:xeJDBC:Username=계정명JDBC:password=비밀번호
EX)jdbc.driverClassName=oracle.jdbc.driver.OracleDriverjdbc.url=jdbc:oracle:thin:@localhost:1521:xejdbc.username=CGVjdbc.password=1234

- MS-SQLJDBC.driverClassName=com.microsoft.sqlserver.jdbc.SQLServerDriverJDBC.url=jdbc:sqlserver://로컬명;DatabaseName="DB명"JDBC.Username=계정명JDBC.password=비밀번호
Ex)jdbc.driverClassName=com.microsoft.sqlserver.jdbc.SQLServerDriverjdbc.url=jdbc:sqlserver://127.0.0.1;DatabaseName=ACCELERPjdbc.username=CGVjdbc.password=1234
```

