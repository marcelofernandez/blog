---
author: Marcelo
category:
  - codear
  - programación
  - python
  - sysadmin
  - ubuntu-ar
date: "2008-10-21T11:57:00+00:00"
guid: https://blog.marcelofernandez.info/?p=112
title: Cómo Conectarse a Oracle desde Python en Ubuntu
url: /2008/10/como-conectarse-a-oracle-desde-python-en-ubuntu/

---
[![](http://1.bp.blogspot.com/_nDZ247g0qSM/SP5jz2hVtRI/AAAAAAAABQM/VE8eMfaV6eg/s320/python-powered-w-200x80.png)](http://www.python.org/) Luego de luchar (y bastante) para acceder a una [BD Oracle](http://www.oracle.com/database/index.html) desde [Python](http://www.python.org/), me propuse escribir los pasos a realizar así quedan para la posteridad. Tengo entendido que el único driver que funciona bien para conectarse es el [cx\_Oracle](http://python.net/crew/atuining/cx_Oracle/), así que voy a tratar de explicar cómo se instala todo desde el comienzo, usando Ubuntu Hardy 8.04.

**Actualización**: este método aún sirve sobre Ubuntu Lucid 10.04 y cx\_Oracle 5.03.

1\. Instalación del Oracle InstantClient
Primero, hay que descargar el [Oracle InstantClient](http://www.oracle.com/technology/tech/oci/instantclient/index.html) para la plataforma Linux que se utilice (x86, x86\_64, [Power](http://www-03.ibm.com/technology/power/), [Itanium](http://www.intel.com/design/itanium/manuals/iiasdmanual.htm), etc.) desde [acá](http://www.oracle.com/technology/software/tech/oci/instantclient/index.html) (es necesario registrarse, pero es gratuito). Los paquetes necesarios son "Instant Client Package - Basic Lite" y "Instant Client Package - SDK". cx\_Oracle debería funcionar con cualquiera de las versiones 9.x y 10.x, así que vamos a bajar la última versión de la serie 10.x (que en este momento es la 10.2.0.4).

Primero hay que descomprimir los 2 archivos .zip; por ejemplo:

```
marcelo@ubuntu-server:~$ unzip basiclite-10.2.0.4.0-linux-x86_64.zip
Archive:  basiclite-10.2.0.4.0-linux-x86_64.zip
inflating: instantclient_10_2/BASIC_LITE_README
[...]
inflating: instantclient_10_2/libocijdbc10.so
inflating: instantclient_10_2/ojdbc14.jar

marcelo@ubuntu-server:~$ unzip sdk-10.2.0.4.0-linux-x86_64.zip
Archive:  sdk-10.2.0.4.0-linux-x86_64.zip
creating: instantclient_10_2/sdk/
creating: instantclient_10_2/sdk/include/
inflating: instantclient_10_2/sdk/include/occi.h
inflating: instantclient_10_2/sdk/include/occiCommon.h
[...]
inflating: instantclient_10_2/sdk/SDK_README
extracting: instantclient_10_2/sdk/ottclasses.zip
inflating: instantclient_10_2/sdk/ott
marcelo@ubuntu-server:~$
```

Al descomprimir ambos archivos en el mismo directorio se generó el nuevo directorio "instantclient\_10\_2" con todo el contenido:

```
marcelo@ubuntu-server:~$ cd instantclient_10_2/
marcelo@ubuntu-server:~/instantclient_10_2$ ls -l
total 33504
-rw-rw-r-- 1 marcelo marcelo      238 2008-03-12 04:37 BASIC_LITE_README
-r--r--r-- 1 marcelo marcelo  1609607 2008-03-12 04:37 classes12.jar
-rwxrwxr-x 1 marcelo marcelo    67542 2008-03-12 04:37 genezi
-rwxrwxr-x 1 marcelo marcelo 21038613 2008-03-12 04:37 libclntsh.so.10.1
-r-xr-xr-x 1 marcelo marcelo  3796601 2008-03-12 04:37 libnnz10.so
-rwxrwxr-x 1 marcelo marcelo  1664116 2008-03-12 04:37 libocci.so.10.1
-rwxrwxr-x 1 marcelo marcelo  4351321 2008-03-12 04:37 libociicus.so
-r-xr-xr-x 1 marcelo marcelo   138033 2008-03-12 04:37 libocijdbc10.so
-r--r--r-- 1 marcelo marcelo  1555682 2008-03-12 04:37 ojdbc14.jar
drwxrwxr-x 4 marcelo marcelo     4096 2008-03-12 04:37 sdk
marcelo@ubuntu-server:~/instantclient_10_2$
```

Creamos el directorio /opt/oracle, y después lo movemos al nuevo directorio renombrado como /opt/oracle/instantclient.

```
marcelo@ubuntu-server:~$ sudo mkdir -p /opt/oracle/
marcelo@ubuntu-server:~$ sudo mv instantclient_10_2 /opt/oracle/instantclient
```

Ahora debemos decirle al linker de bibliotecas dinámicas del sistema ( [ld](http://www.math.utah.edu/docs/info/ld_toc.html)) la ubicación de un nuevo path donde podrá encontrar más bibliotecas software para el programa que lo requiera; en este caso ese "programa" será el módulo de python para acceder a Oracle, cx\_Oracle. Es decir, que además de las bibliotecas disponibles en, por ejemplo, /usr/lib y /usr/local/lib, también pondremos a disposición de los programas del usuario las bibliotecas del directorio /opt/oracle/instantclient. Esto se hace con el programa [ldconfig](http://linux.die.net/man/8/ldconfig):

```
marcelo@ubuntu-server:~$ sudo -i
[sudo] password for marcelo:
root@ubuntu-server:~# echo "/opt/oracle/instantclient" > /etc/ld.so.conf.d/oracle.conf
root@ubuntu-server:~# ldconfig
root@ubuntu-server:~# ldconfig --print | grep /opt/oracle
libocijdbc10.so (libc6,x86-64) => /opt/oracle/instantclient/libocijdbc10.so
libociicus.so (libc6,x86-64) => /opt/oracle/instantclient/libociicus.so
libocci.so.10.1 (libc6,x86-64) => /opt/oracle/instantclient/libocci.so.10.1
libnnz10.so (libc6,x86-64) => /opt/oracle/instantclient/libnnz10.so
libclntsh.so.10.1 (libc6,x86-64) => /opt/oracle/instantclient/libclntsh.so.10.1
root@ubuntu-server:~# cd /opt/oracle/instantclient
root@ubuntu-server:/opt/oracle/instantclient# ln -s libclntsh.so.10.1 libclntsh.so
root@ubuntu-server:/opt/oracle/instantclient# ln -s libocci.so.10.1 libocci.so
```

La idea es crear un archivo con extensión .conf en el directorio /etc/ld.so.conf.d/ \[1\], con un nombre relacionado con nuestro propósito, que sólo indica un nuevo directorio de bibliotecas de ubicación "no estándar".

Luego hay que refrescar la información de bibliotecas, con el comando "ldconfig". Después, "ldconfig --print" muestra todas las bibliotecas disponibles en el sistema para enlazar dinámicamente, pero como ahora sólo son de nuestro interés las del directorio /opt/oracle, se filtra la salida con un grep acorde.

También hacemos dos enlaces simbólicos a las bibliotecas principales del Instantclient, cuyo número de versión es "genérico", a diferencia de los archivos originales, cuya versión es 10.1. Al momento de compilar el cx\_Oracle, éste buscará la versión genérica de estas librerías, no una versión específica (por ende, serán los archivos libclntsh.so y libocci.so). Todo esto debe hacerse como root, por eso el "sudo -i" inicial.

2\. Instalación del cx\_Oracle(instrucciones extraídas en parte y basadas del [Readme.txt](http://python.net/crew/atuining/cx_Oracle/README.txt) del proyecto)

Una vez que se tiene el Instantclient instalado y configurado, vamos a proceder a instalar el módulo cx\_Oracle. Lo "complicado" de la instalación del módulo (a diferencia de la gran mayoría que son 100% python) es que requiere correr un proceso de compilación contra las bibliotecas Instantclient que acabamos de instalar. Lo bueno es que a esta altura ya hicimos casi todo el trabajo. :-)

Primero instalamos algunas dependencias:

```
marcelo@ubuntu-server:~$ sudo apt-get install python-dev python-setuptools build-essential
Leyendo lista de paquetes... Hecho
Creando árbol de dependencias
Leyendo la información de estado... Hecho
Se instalarán los siguientes paquetes extras:
binutils dpkg-dev g++ g++-4.2 gcc gcc-4.2 libc6-dev libgomp1 libstdc++6-4.2-dev libtimedate-perl linux-libc-dev make patch python-pkg-resources python2.5-dev
Paquetes sugeridos:
binutils-doc debian-keyring g++-multilib g++-4.2-multilib gcc-4.2-doc libstdc++6-4.2-dbg autoconf automake1.9 bison flex gcc-doc gcc-multilib gdb libtool manpages-dev
gcc-4.2-locales gcc-4.2-multilib libgcc1-dbg libgomp1-dbg libmudflap0-4.2-dbg libmudflap0-4.2-dev glibc-doc libstdc++6-4.2-doc make-doc diff-doc
Se instalarán los siguientes paquetes NUEVOS:
binutils build-essential dpkg-dev g++ g++-4.2 gcc gcc-4.2 libc6-dev libgomp1 libstdc++6-4.2-dev libtimedate-perl linux-libc-dev make patch python-dev python-pkg-resources
python-setuptools python2.5-dev
0 actualizados, 18 se instalarán, 0 para eliminar y 0 no actualizados.
Necesito descargar 12,8MB de archivos.
After this operation, 52,9MB of additional disk space will be used.
¿Desea continuar [S/n]?
[...]
```

Cuando se compilan módulos de python que acceden a librerías nativas (generalmente en C), debe instalarse el paquete "python-dev", que contiene los headers de las estructuras, funciones y demás símbolos del lenguaje. Las setuptools creo que no hacen falta, pero siempre es útil tenerlo para instalar más módulos después. El metapaquete "build-essential" nos provee todo lo básico necesario para compilar programas en C/C++.

Luego descargamos y descomprimimos el código fuente del módulo, en formato .tar.gz (hay RPMs y paquetes para Windows, pero no DEBs). La última versión es la [4.3.1](http://prdownloads.sourceforge.net/cx-oracle/cx_Oracle-4.3.1.tar.gz?download). La descompresión puede ser en un directorio cualquiera, como por ejemplo, el home de un usuario (no es necesario root):

```
marcelo@ubuntu-server:~$ tar xvzf cx_Oracle-4.3.1.tar.gz
cx_Oracle-4.3.1/
cx_Oracle-4.3.1/test/
cx_Oracle-4.3.1/test/test_dbapi20.py
cx_Oracle-4.3.1/test/LongVar.py
cx_Oracle-4.3.1/test/LobVar.py
cx_Oracle-4.3.1/test/DateTimeVar.py
cx_Oracle-4.3.1/test/test.py
cx_Oracle-4.3.1/test/NumberVar.py
[...]
marcelo@ubuntu-server:~$ cd cx_Oracle-4.3.1/
marcelo@ubuntu-server:~/cx_Oracle-4.3.1$
```

Luego, para poder compilar/construir primero e instalar después el módulo, hay que establecer la variable de entorno ORACLE\_HOME:

```
marcelo@ubuntu-server:~/cx_Oracle-4.3.1$ export ORACLE_HOME=/opt/oracle/instantclient/
```

Ahora, a construir el módulo. Puede verse que no hace falta ser root:

```
marcelo@ubuntu-server:~/cx_Oracle-4.3.1$ python setup.py build
running build
running build_ext
building 'cx_Oracle' extension
gcc -pthread -shared -Wl,-O1 -Wl,-Bsymbolic-functions build/temp.linux-x86_64-2.5/cx_Oracle.o -L/opt/oracle/instantclient/lib -L/opt/oracle/instantclient/ -lclntsh -o build/lib.linux-x86_64-2.5/cx_Oracle.so
marcelo@ubuntu-server:~/cx_Oracle-4.3.1$
```

Listo, ya podemos instalar y probar el nuevo módulo:

```
marcelo@ubuntu-server:~/cx_Oracle-4.3.1$ sudo python setup.py install
running install
running build
running build_ext
running install_lib
copying build/lib.linux-x86_64-2.5/cx_Oracle.so -> /usr/lib/python2.5/site-packages
running install_egg_info
Writing /usr/lib/python2.5/site-packages/cx_Oracle-4.3.1.egg-info
marcelo@ubuntu-server:~/cx_Oracle-4.3.1$ ipython
Python 2.5.2 (r252:60911, Jul 31 2008, 17:31:22)
Type "copyright", "credits" or "license" for more information.

IPython 0.8.1 -- An enhanced Interactive Python.
?       -> Introduction to IPython's features.
%magic  -> Information about IPython's 'magic' % functions.
help    -> Python's own help system.
object? -> Details about 'object'. ?object also works, ?? prints more.

In [1]: import cx_Oracle

In [2]: conn_str='scott/tiger@192.168.1.90:1521/DESARROLLO'

In [3]: db_conn = cx_Oracle.connect(conn_str)

In [4]: cursor = db_conn.cursor()

In [5]: cursor.execute('SELECT username, user_id, created FROM dba_users')
Out[5]:
[,
,
 ]

In [6]: registros = cursor.fetchall()

In [7]: for r in registros:
...:     print str(r)
...:
...:
('MGMT_VIEW', 46, datetime.datetime(2008, 10, 15, 13, 4, 47))
('SYS', 0, datetime.datetime(2008, 10, 15, 12, 43, 8))
('SYSTEM', 5, datetime.datetime(2008, 10, 15, 12, 43, 8))
('DBSNMP', 24, datetime.datetime(2008, 10, 15, 12, 51, 3))
('SYSMAN', 44, datetime.datetime(2008, 10, 15, 13, 3, 24))
('SCOTT', 49, datetime.datetime(2008, 10, 21, 17, 30, 43))
('PYTK08', 47, datetime.datetime(2008, 10, 15, 13, 54, 37))
('OUTLN', 11, datetime.datetime(2008, 10, 15, 12, 43, 13))
('MDSYS', 43, datetime.datetime(2008, 10, 15, 12, 57, 21))
('ORDSYS', 40, datetime.datetime(2008, 10, 15, 12, 57, 21))
('ANONYMOUS', 36, datetime.datetime(2008, 10, 15, 12, 55, 58))
('EXFSYS', 34, datetime.datetime(2008, 10, 15, 12, 55, 41))
('WMSYS', 25, datetime.datetime(2008, 10, 15, 12, 51, 55))
('XDB', 35, datetime.datetime(2008, 10, 15, 12, 55, 58))
('ORDPLUGINS', 41, datetime.datetime(2008, 10, 15, 12, 57, 21))
('SI_INFORMTN_SCHEMA', 42, datetime.datetime(2008, 10, 15, 12, 57, 21))
('DIP', 19, datetime.datetime(2008, 10, 15, 12, 46))
('TSMSYS', 21, datetime.datetime(2008, 10, 15, 12, 49, 19))

In [8]:
```

Listo! Ya tenemos acceso a Oracle desde mi lenguaje de programación preferido, Python.

Espero que les sirva.

Saludos!
Marcelo

\[1\] Esto es así en Ubuntu Hardy 8.04. En versiones más antiguas de Ubuntu, como Dapper 6.06, este directorio no existe y en su lugar se debe agregar una línea con el mismo contenido pero en el archivo /etc/ld.so.conf (creándolo en caso de no existir).
