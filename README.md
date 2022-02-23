Despliegue de un servidor sonar(version latest) utilizando docker-compose para personalizacion

1.-Requisitos: 
Para poder configurar un job de test con maven(con java11 en la makina host linux) he instalado sdkman y e instalado maven:

curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
sdk version
sdk install maven

2.-Creacion del proyecto de test(maven)

2.1-en la carpeta java-project tienes un proyecto test de maven creada con:(son 1 linea aunque parezcan 2)

mvn archetype:generate -DgroupId=com.example.project1 -DartifactId=java-project -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

2.2.-y con lo siguiente creas el jobs en el sonar:

mvn clean verify sonar:sonar -Dsonar.login=0b7713b8fca57828090bd05c743ea54a6fa4c2a7

nota: el token lo sacas de: sonar.. administration.. security.. users.. escoges el user y a la derecha tienes: 
nota2: mas abajo te pongo el contenido de 2 ficheros a tener en cuenta: 1 el pom.xml del proyecto (lo crea con el primer paso 2.1.- de crear proyecto)  el settings.xml de la carpeta: ~/.m2

3.- Contenido de las carpetas: Personalizado docker-compose para que utilice carpetas dentro de la carpeta de sonara para tener a mano, logs, plugins, etc

/opt/sonarqube/data: data files, such as the embedded H2 database and Elasticsearch indexes
/opt/sonarqube/logs: contains SonarQube logs about access, web process, CE process, Elasticsearch logs
/opt/sonarqube/extensions: for 3rd party plugins (para guardar-instalar los plugins (en formato jar) tiene que existir la carpeta plugins si no existe .. creala)

4.- Configracion de ficherosficheros: 
4.-1 pom.xml del proyecto:
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.example.project1</groupId>
  <artifactId>java-project</artifactId>
  <packaging>jar</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>java-project</name>
  <url>http://maven.apache.orgur</url>
  <properties>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>

4.2.-settins.xml de ~/.m2
<settings>
    <pluginGroups>
        <pluginGroup>org.sonarsource.scanner.maven</pluginGroup>
    </pluginGroups>
    <profiles>
        <profile>
            <id>sonar</id>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
            <properties>
                <!-- Optional URL to server. Default value is http://localhost:9000 -->
                <sonar.host.url>
                  http://localhost:9001
                </sonar.host.url>
            </properties>
        </profile>
     </profiles>
</settings>
