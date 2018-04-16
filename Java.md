<p align="center"><img src="img/java.png" title="Java_logo"></p>

## Instalación.
### Eliminar OpenJDK.
`# apt-get purge openjdk-\*`
### Edita el archivo PATH.
`# gedit /etc/profile`
```bash
#Java
JAVA_HOME=/opt/java/jdk1.8.0_162
PATH=$PATH:$HOME/bin:$JAVA_HOME/bin
export JAVA_HOME
export PATH
```
### Informa al sistema dónde está Java.
`# update-alternatives --install "/usr/bin/java" "java" "/opt/java/jdk1.8.0_162/bin/java" 1`

`# update-alternatives --install "/usr/bin/javac" "javac" "/opt/java/jdk1.8.0_162/bin/javac" 1`

`# update-alternatives --install "/usr/bin/javaws" "javaws" "/opt/java/jdk1.8.0_162/bin/javaws" 1`
### Informa al sistema que Java debe ser predeterminado.
`# update-alternatives --set java /opt/java/jdk1.8.0_162/bin/java`

`# update-alternatives --set javac /opt/java/jdk1.8.0_162/bin/javac`

`# update-alternatives --set javaws /opt/java/jdk1.8.0_162/bin/javaws`
### Revisa si Java se instaló correctamente.
`# source /etc/profile`

`$ java -version`

`$ javac -version`
## Básico.
## Intermedio.
## Avanzado.
## Buenas practicas.
## Java EE.