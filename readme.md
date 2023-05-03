# REST-Grundlagen

## Motivation: Microservices

Microservices ...
 - sind verteilte Anwendungen.
 - sind zustandslos.
 - kommunizieren technologieunabhängig miteinander (HTTP)
 - kommunizieren direkt/synchron oder indirekt/asynchron miteinander.
 - haben gegenüber monolithischenen Anwendungen Vorteile und Herausforderungen
   - schnellere Implementierung, einfachere Tests des Einzelnen
   - ABER komplexer in der Gesamtstruktur
   - Wartungsaufwände evtl. größer
   - Verwaltung der Abhängigkeiten untereinander, Überblick über Gesamtnetzwerk
   - Resilience Patterns: Ausfall des einzelnen Services führt nicht zu Ausfall des gesamten Netzwerks
   - Observability (Logging, Tracing, ...)
 - verlagern Problemstellungen (z.B. Abhängigkeiten untereinander) von innen nach außen (in die Netzwerke)
 - sind von Vorteil bei der agilen Entwicklung:
   - kleinere, unabhängigere Teams
   - unabhängige Releases/Deployments
   - mehr Entscheidungen auf Teamebene (Programmiersprache/Framework, Branching-Modell, ...)
 - nutzen Cloud-Technologien besser (Skalierung einzelner Services effizienter)
 - sind in der Cloud besser zu betreiben (Automatisierung der Skalierung, Containerisierung, Bereitstellung von Datenbanken u.a. Abhängigkeiten)
 
Die Kommunikation untereinander erfordert ein klares Schnittstellen-Design.
 - Bewusstsein über die Bedeutung der Schnittstelle (API First)
 - Versionierung, Kompatibilitäten (API Management)
 - evtl. Katalogisierung mehrerer Services mit gleicher Schnittstelle

## REST-Schnittstellen

 - Auf Basis von HTTP
 - Best Practice
 - Vereinheitlichung
 
### Grundlegende Regeln

 - URLs für Ressourcen ("Daten")
   - Substantive
   - Listenressourcen (Mehrzahl) vs. Einzelressourcen
   - Subressourcen (URLs "verlängern" sich)
 - HTTP-Methoden
   - Abbildung der CRUD-Operationen
     - (C) POST (oder PUT)
     - (R) GET
     - (U) PUT, PATCH
     - (D) DELETE
     - GET, PUT, DELETE sind idempotent
       - POST zum Anlegen, wenn Primärschlüssel serverseitig generiert wird
       - PUT zum Anlegen, wenn der Primärschlüssel über den Request mitkommt
       - PATCH kann idempotent sein, muss aber nicht
   - Semantische Unterscheidung von POST und PUT
     - POST = Übergebe Daten an eine Ressource, die ihre eigene Logik damit ausführt
       - Listenressource erzeugt neues Item
       - Aktivitätsressourcen
     - PUT = Ersetze die Ressource durch die übergebenen Daten
 - HTTP Statuscodes
   - Gruppierung in Nummernkreise
     - 2xx für Erfolg
     - 4xx für Anfragefehler
     - 5xx für Serverinterne Fehler (nicht Teil des API Designs)
   - Wichtigste Codes
     - Erfolgsfälle
       - 200: OK (mit Body)
       - 201: Created (mit `Location`-Header, falls neu erzeugte Ressource eine andere URL hat)
       - 202: Accepted (Asynchrone Verarbeitung)
       - 204: No Content (ohne Body)
     - Clientseitige Fehler
       - 400: Bad Request (allgemeiner Fehler)
       - 404: Not Found (Resource nicht vorhanden)
       - Content Negotiation
         - 406: Not Acceptable (Response mit gewünschtem Format nicht produzierbar)
         - 415: Unsupported Mediatype (Request-Body-Format wird nicht unterstützt)
       - Validierung
         - 400: Bad Request (Verwendung wird empfohlen)
         - 422: Unprocessable Content (falls Validierungsfehler speziell vom 400er unterschieden werden sollen)
       - Security
         - 401: Unauthorized ("Ich weiß nicht, wer Du bist.")
         - 403: Forbidden ("Ich weiß, wer Du bist, aber Du darfst nicht.")
 - Content Negotiation
   - HTTP `Accept`-Header mit Wunschliste
   - HTTP `Content-Type`-Header zur Beschreibung des Body
- Versionierung (Semantic Versioning) über URL
  - `/api/v1/customers` vs `/api/v2/customers`
- Guidelines
    - HUK HSA Makroarchitektur
    - Beispiel: [Zalando API Guidelines](https://opensource.zalando.com/restful-api-guidelines/#)

### Beschreibung

- [Standardisiertes Format (OpenAPI) + Tools (Swagger)](https://swagger.io/docs/specification/about/)
  - Austauschformat zwischen Editoren

### Tooling

- Generatoren
    - OpenAPI -> Code (Swagger CodeGen)
    - Code -> OpenAPI (Framework-spezifisch)
- Editoren
  - [Swagger Editor](https://editor.swagger.io/)
  - IntelliJ hat eingebauten Editor
- Testen
  - Swagger UI (manuelles Testen)
    - Integriert im Editor
    - Kann über Dependency auch in Quarkus/Spring-Boot-App integriert werden
  - IntelliJ HTTP Client (manuelles Testen)
    - zu finden im Menü `Tools`
    - Erstellen und Versenden von HTTP-Anfragen
    - Speichern in IntelliJ-spezifischen Dateien
  - Postman (manuelles + automatisiertes Testen)
    - Import von OpenAPI -> Generierung von Request Sample
    - Verwaltung von Umgebungen (Hosts, Variablen...)
    - Implementieren von Tests (JS)
    - Automatisierung möglich (Newman)
    - [Tutorial](https://www.testautomatisierung.org/rest-api-tests-mit-postman/)
  - `curl` (Kommandozeilenbefehl, auch für Automatisierung)
    - ebenfalls als Austauschformat zwischen den Tools geeignet
      (Swagger UI, PostMan, IntelliJ HTTP Client)

## Weiterführende Themen

- [Patterns: BFF, API Gateway](https://www.manuelkruisz.com/blog/posts/api-gateway-vs-bff)
- [Hypermedia APIs](https://www.mscharhag.com/api-design/hypermedia-rest)
  - [HAL](https://stateless.group/hal_specification.html)
    - [HAL Explorer](https://toedter.github.io/hal-explorer/release/reference-doc/)
    - [HAL9000 (kleine Anekdote drumherum)](https://de.wikipedia.org/wiki/HAL_9000)
    - [Quarkus-Support](https://quarkus.io/guides/resteasy#links)
- [RFC-7807: ProblemDetails](https://www.linkedin.com/pulse/rfc-7807-error-handling-standard-apis-david-rold%C3%A1n-mart%C3%ADnez/)
- Andere Arten von Webservices
  - [SOAP (W3C)](https://www.tutorialspoint.com/soap/soap_examples.htm)
  - [GraphQL](https://blog.logrocket.com/graphql-vs-rest-api-why-you-shouldnt-use-graphql)
    - [Spring Boot Sample](https://github.com/ueberfuhr-trainings/spring-features/blob/main/docs/graphql/index.md)
