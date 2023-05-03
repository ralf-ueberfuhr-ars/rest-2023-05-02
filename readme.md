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
 - HTTP-Methoden für CRUD-Operationen
   - (C) POST (oder PUT)
   - (R) GET
   - (U) PUT, PATCH
   - (D) DELETE
   - GET, PUT, DELETE sind idempotent
     - POST zum Anlegen, wenn Primärschlüssel serverseitig generiert wird
     - PUT zum Anlegen, wenn der Primärschlüssel über den Request mitkommt
     - PATCH kann idempotent sein, muss aber nicht
 - HTTP Statuscodes
   - 2xx für Erfolg
   - 4xx für Anfragefehler
   - 5xx für Serverinterne Fehler (nicht Teil des API Designs)
 - Content Negotiation
   - HTTP `Accept`-Header mit WUnschliste
   - HTTP `Content-Type`-Header zur Beschreibung des Body
 