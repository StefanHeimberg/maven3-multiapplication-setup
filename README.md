# example-multiapplication-setup
Beispielkonfiguration Maven Multi-Application &amp; Multi-Module Setup

Dieses Beispiel soll veranschaulichen, wie in einem grösseren Umfeld mit mehreren Applikationen ein Maven Setup aussehen könnte. Ich habe diese Repository erstellt um eine Diskussion bei meinem aktuellen Arbeitgeber zu initiieren.


Folgendes ist zu diesem Beispiel zu sagen:
* Es gibt keinen Sourcecode. Nur das Maven Projekt Model (pom.xml)
* Alles innerhalb diese git repositories. In der realen Welt würde man für diese Konstellation mit sicherheit separate git repositories verwenden. Wie z.B.:
    * framework.git
    * interfaces.git
    * applikation1.git
    * applikation2.git
* Zyklische Abhängigkeiten zwischen den Applikation


## Applikationslandschaft

Die Applikationslandschaft ist fiktiv und soll nur eine Ausgangslage sein, um zu verstehen wieso einige dependencies  untereinander existieren.

Mir geht es nur darum, dass die einzelnen Applikation "etwas" zu tun haben, und es unter ihnen "zyklische" dependencies geben könnte.

z.B.:
* Applikation A braucht Applikation B
* Applikation B braucht Applikation A


### Framework

In einem grösseren Umfeld gehe ich davon aus, dass es gemeinsamkeite zwischen den Applikationen gibt. Häuffig sind so genannte "Frameworks" anzutreffen.

Diese Framework bietet die Basisplatform für alle zu erstellenden Business Applikationen. 


### Applikation A

Die Applikation A ist eine klassische Webapplikation welche in bekannte Schichten unterteilt ist.
Ein Teil der Business-Operationen wird von der Webapplikation verwendet, und ein anderer Teil ist über eine Webservice Schnittstelle zugänglich.

Die Webapplikation selbst, verwendet auf der Client Seite für einige Request eine REST Schnittstelle der Applikation B

Das Framework wird verwendet für:
* Logging (Splunk)
* Unit Testing
* Persistenz (DAO Helper, EntityManager helpers etc...)
* Publizieren der SOAP Webservice Schnittstellen aus dem Business. Logging Handler etc...
* Helper für die view, um den JavaScript code zu erstellen welcher im Client für die SOAP request verwendet wird. (ich weiss, etwas überflüssig aber ist ja nur ein beispiel)


### Applikation B

Die Applikation B liefert nur REST Schnittstellen welche von anderen konsumiert werden können. Für die Business Logik ist es ebenfalls notwendig daten aus der Applikation A über die SOAP Webservice Schnittstelle abzurufen.

Applikation B verwendet zusätzlich noch google guava welches nicht vom Framework geliefert wird.

Das Framwork wird verwendet für:
* Logging (Splunk )
* Unit Testing
* Persistenz (DAO Helper, EntityManager helpers etc...)
* Konsumieren der SOAP Webservice Schnittstellen der Applikation A. Logging Handler etc...
* Publizieren der REST Webservice Schnittstellen. Logging Handler etc...


### Interfaces

Diese Projekt beinhalte alle wsdls, xml, xsd, etc... könnte auch aus mehreren maven module bestehen.

Das Interfaces Projekt ist da, um die zyklischen Dependencies zwischen den Applikationen aufzuheben. Ist aber auch nur notwendig wenn hier z.B. die generierten Jax-B Klassen des Webservice / REST Schnittstellen sind. Besser wäre es noch die jeweilige generierung in den Applikationen selbst zu machen, somit müsste man nur die wsds/xsd etc... in die Applikationen kopieren (http://docs.spring.io/spring-ws/site/reference/html/why-contract-first.html)


## Reihenfolge im Build System

* Framework (mvn clean install -f Framework/pom.xml)
* Interfaces (mvn clean install -f Interfaces/pomx.ml)
* Applikation A oder Applikation B (mvn clean install -f Applikation?/pom.xml)
