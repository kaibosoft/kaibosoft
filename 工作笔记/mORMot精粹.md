    mORMot的下载地址是https://codeload.github.com/synopse/mORMot/zip/master，作者的几乎每天都有更新，所以大家最好还是学会SVN或者GIT的用法，保证代码的最新。
    GIT下载命令 git clone https://github.com/synopse/mORMot.git --depth 1
    以下是mORMot的简单介绍：
    1. SynCommons mORMot核心单元，包括编码转换，文件读取，线程管理，内存管理，RTTI处理等功能,常用以下两类方法：
    　　1. 实现ansi,wide,utf8,hex,variant之间的相互转化,URL编码解码，base64编码解码。
    　　2. AnyTextFileToString,AnyTextFileToRawUTF8，AnyTextFileToSynUnicode读取文件，目录删除，读取文件时间等功能。
    2. SynDb mORMot数据处理核心单元。实现记录集与JSON对象，记录集与RTTI对象的相互转化。
    3. SynCrtSock，实现mORMot的基本Socket操作。SynBidirSock实现基于mORMot的WebSocket处理操作，实现基于WebSocket的JSON，BIN通讯功能。
    4. 数据处理单元：SynOleDB处理jet,mssql数据库,SynSQLite3、SynSQLite3Static、SynDBSQLite3处理SQLITE3数据库，可以不带DLL运行。SynDBRemote单元可以将连接变为远程连接，让客户实现远程HTTP数据库调用。SynDBZeos使用ZEOS库连接数据库，同样的还有SynDBDataset目录下的UNIDAC,FIREDAC，BDE等连接方式。
    5. 其它单元：SynLog日志管理单元。SynLz,SynLzo实现Lz压缩解压缩处理.SynDBMidasVCL继承于TClientDataSet的记录集控件。
    以上单元仅是mORMot的基本处理单元，mORMot的ORM框架实际上使用的是SQLite3目录下的单元。
***
    1. 使用Sqlite3数据库，引用SynCommons, SynDB,SynDBSQLite3, SynSQLite3, SynSQLite3Static，连接为 gProps := TSQLDBSQLite3ConnectionProperties.Create('test.db3', '', '', '用户密码');
    2. 使用ZEOS可以连接不同数据库，引用SynCommons, SynDB,SynDBZeos
        1. 连接FireBird:gProps := TSQLDBZEOSConnectionProperties.Create('zdbc:firebird-2.0://127.0.0.1:3050/model?username=sysdba;'+'password=masterkey;LibLocation=fbclient.dll', '', '', '');
        2. 连接MySql:gProps := TSQLDBZEOSConnectionProperties.Create('zdbc:mysql://127.0.0.1:3306/model?username=sysdba;'+'password=masterkey;LibLocation=libmysql.dll', '', '', '');
        3. 连接MSSQL：dbConn := TOleDBMSSQLConnectionProperties.Create(cServer,cDatabase,cUserId,cUserPwd);