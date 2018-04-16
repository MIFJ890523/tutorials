<p align="center"><img src="img/maven.png" title="maven_logo">

## Instalación
### Descarga Maven
Descargar Apache Maven, descomprimir y comocar en una carpeta del sistema "/opt/maven"

https://maven.apache.org/download.cgi
### Edita el archivo PATH
`$ gedit .profile`
```bash
#Maven
export MAVEN_HOME=/opt/maven
export PATH=$PATH:$MAVEN_HOME/bin
```
### Revisa si Maven se instaló correctamente
`$ . .profile`

`$  mvn --version`
## Creando proyecto Maven
Se genera un proyecto nuevo con los siguientes datos:

`$ mvn archetype:generate -DgroupId=unam.fesi.colector -DartifactId=colector -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`


El significado de los parámetros es el siguiente:

* "archetype" genera un nuevo proyecto a partir de un arquetipo

* "groupId" es un identificador para agrupar varios proyectos que estén relacionados.

* "artifactId" es el nombre del proyecto y será también el nombre del jar que se genere.

* "archetypeArtifactId" esto le dice a Maven que cree un proyecto Java a partir de esta plantilla

* "interactiveMode" le indica a Maven que use los valores pasados desde la línea de comando

## Ejecutando proyecto Maven
Se ejecuta el proyecto con el siguiente comando:

`$ mvn compile exec:java -Dexec.mainClass="unam.fesi.colector.App"`

* "exec.mainClass" Indica la clase Main o clase principal a ejecutar

## pom.xml
POM significa modelo de objeto de proyecto (project object model). Es una representación XML de un proyecto Maven.
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>unam.fesi.colector</groupId>
  <artifactId>colector</artifactId>
  <packaging>jar</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>colector</name>
  <url>http://maven.apache.org</url>
  <build>
    <plugins>
    </plugins>
  </build>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```

* "packaging" se indica el tipo de empaquetado que hay que hacer con el proyecto. Podemos usar jar, war, ear, pom.

* "version" se indica la versión del proyecto con la que estamos trabajando. Al indicar SNAPSHOT se quiere decir que es una versión evolutiva, es decir que estamos trabajando para obtener al versión 1.0

## Plugins
Maven es en esencia, un marco de ejecución de plugins, todo el trabajo se realiza mediante plugins.

* ### assembly
El plugin de "assembly" para Maven está destinado principalmente a permitir a los usuarios agregar el resultado del proyecto junto con sus dependencias, módulos, documentación del sitio y otros archivos en un único archivo distribuible

`$ mvn clean compile assembly:single`
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-assembly-plugin</artifactId>
	<version>3.1.0</version>
    <configuration>
        <archive>
            <manifest>
                <mainClass>mx.interware.ssh.App</mainClass>
            </manifest>
        </archive>
        <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
        </descriptorRefs>
    </configuration>
</plugin>
```

* ### exec
El complemento proporciona 2 objetivos para ayudar a ejecutar el sistema y los programas de Java.

`$ mvn exec:exec` ejecuta programas y programas Java en un proceso separado.

`$ mvn exec:java` ejecuta programas Java en la misma máquina virtual.
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-jar-plugin</artifactId>
    <version>3.0.2</version>
    <configuration>
        <archive>
            <manifest>
                <addClasspath>true</addClasspath>
                <mainClass>mx.interware.ssh.App</mainClass>
            </manifest>
        </archive>
    </configuration>
</plugin>
```

* ### jar
Este complemento proporciona la capacidad de construir archivos jar

`$ mvn jar:jar` crea un archivo jar para los recursos inclusivos de las clases de proyectos.

`$ mvn jar:test-jar` crea un archivo jar para tus clases de prueba de proyecto.
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-jar-plugin</artifactId>
    <version>3.0.2</version>
    <configuration>
        <archive>
            <manifest>
                <addClasspath>true</addClasspath>
                <mainClass>mx.interware.ssh.App</mainClass>
            </manifest>
        </archive>
    </configuration>
</plugin>
```

## Dependencias
Uno de los puntos fuertes de maven son las dependencias. En nuestro proyecto podemos decirle a maven que necesitamos un jar (por ejemplo, log4j o el conector de MySQL) y maven es capaz de ir a internet, buscar esos jar y bajárselos automáticamente. Es más, si alguno de esos jar necesitara otros jar para funcionar, maven "tira del hilo" y va bajándose todos los jar que sean necesarios.

https://mvnrepository.com/

```xml
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>8.0.9-rc</version>
</dependency>
```
## Repositorio local
¿Dónde deja maven los jar que se baja?. Por defecto, en el home del usuario, crea un subdirectorio ".m2/repository" y ahí va dejando todos los jar que se va bajando de internet.

Si dentro de nuestro ordenador tenemos varios proyectos maven y queremos usar los jar que hemos generado en uno de ellos en otro de los proyectos, podemos subir esos jar al repositorio local. El comando es `$ mvn install`. Este comando compila, pasa los test de JUnit, genera el jar y lo sube a nuestro `$HOME/.m2/repository` con el groupId, artifactId y versión que hayamos decidido cuando creamos el proyecto. En el momento que este jar está en nuestro `$HOME/.m2/repository`, ya está accesible para cualquier otro proyecto maven en el mismo ordenador.
