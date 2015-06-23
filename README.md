# example-multiapplication-setup
Beispielkonfiguration Maven Multi-Application &amp; Multi-Module Setup

Dieses Beispiel soll veranschaulichen, wie in einem grösseren Umfeld mit mehreren Applikationen ein Maven Setup aussehen könnte.

Folgendes ist zu diesem Beispiel zu sagen:
* es gibt keinen code. nur maven pom.xml
* alles innerhalb diese git repositories. in der realen welt wären dies mit sicherheit separate git repositories mindestens wie folgt
    * framework.git
    * interfaces.git
    * applikation1.git
    * applikation2.git

## Appliaktionslandschaft

Die Applikationslandschaft ist fiktiv, und soll nur eine Ausgangslage sein, um zu verstehen wieso einige dependencies existieren.

Mir geht es nur darum, dass die einzelnen Applikation "etwas" zu tun haben, und es unter ihnen "zyklische" dependencies geben könnte.

z.B.:
* Applikation A braucht Applikation B
* Applikation B braucht Applikation A


### Framework

In einem grösseren Umfeld gehe ich davon aus, dass es gemeinsamkeite zwischen den Applikationen gibt. Häuffig sind so genannte "Frameworks" anzutreffen.

Diese Framework bietet die Basis Platform für alle zu erstellenden Business Applikationen. 


### Applikation A

Die Applikation A ist eine Klassische Webapplikation welche in bekannte Schichten unterteilt ist.
Ein Teil der Business-Operationen wird von der Webapplikation verwendet, und ein Anderer Teil ist über eine Webservice Schnittstelle zugänglich.

Die Webapplikation selbst, verwendet auf der Client Seite für einige Request eine REST Schnittstelle der Applikation B

Das Framework wird verwendet für:
* Logging (Splunk )
* Unit Testing
* Persistenz (DAO Helper, EntityManager helpers etc...)
* Publizieren der SOAP Webservice Schnittstellen aus dem Business. Logging Handler etc...
* Helper für die view, um den JavaScript code zu erstellen welcher im Client für die SOAP request verwendet wird. (ich weiss, etwas überflüssig aber ist ja nur ein beispiel)


### Applikation B

Die Applikation B liefert nur REST Schnittstellen welche von anderen konsumiert werden können. Für die Business Logik ist es ebenfalls notwendig daten aus der Applikation A über die SOAP Webservice Schnittstelle abzurufen.

Das Framwork wird verwendet für:
* Logging (Splunk )
* Unit Testing
* Persistenz (DAO Helper, EntityManager helpers etc...)
* Konsumieren der SOAP Webservice Schnittstellen der Applikation A. Logging Handler etc...
* Publizieren der REST Webservice Schnittstellen. Logging Handler etc...


