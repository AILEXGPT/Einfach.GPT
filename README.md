# 🔒 Einfach.PGPT 📑

> Installations- und Nutzungsdokumentation: https://docs.privategpt.dev/

Einfach.PGPT ist ein produktionsbereites KI-Projekt, das es dir ermöglicht, Fragen zu deinen Dokumenten mithilfe der Kraft von Großen Sprachmodellen (LLMs) zu stellen, auch in Szenarien ohne Internetverbindung. 100% privat, keine Daten verlassen deine Ausführungsumgebung zu irgendeinem Zeitpunkt.

Das Projekt bietet eine API, die alle Grundlagen bietet, um private, kontextbewusste KI-Anwendungen zu erstellen. Es folgt und erweitert den [OpenAI API-Standard](https://openai.com/blog/openai-api) und unterstützt sowohl normale als auch Streaming-Antworten.

Die API ist in zwei logische Blöcke unterteilt:

**High-Level-API**, die die gesamte Komplexität einer RAG (Retrieval Augmented Generation) Pipeline-Implementierung abstrahiert:
- Einlesen von Dokumenten: internes Verwalten von Dokumenten-Parsing, Aufteilung, Metadatenextraktion, Erzeugung von Einbettungen und Speicherung.
- Chat & Vervollständigungen unter Verwendung von Kontext aus eingelesenen Dokumenten: Abstraktion der Kontexterfassung, der Prompt-Engineering und der Antwortgenerierung.

**Low-Level-API**, die es fortgeschrittenen Benutzern ermöglicht, ihre eigenen komplexen Pipelines zu implementieren:
- Erzeugung von Einbettungen: basierend auf einem Textstück.
- Kontextbezogene Chunks-Abruf: Bei einer Anfrage werden die relevantesten Textabschnitte aus den eingelesenen Dokumenten zurückgegeben.

Zusätzlich dazu wird ein funktionierender [Gradio UI](https://www.gradio.app/) Client bereitgestellt, um die API zu testen, zusammen mit einer Reihe nützlicher Tools wie Bulk-Model-Download-Skript, Einleseskript, Dokumentenordner-Überwachung usw.

> 👂 **Brauchst du Hilfe bei der Anwendung von Einfach.PGPT für deinen spezifischen Anwendungsfall?**
> [Lass es uns wissen](https://forms.gle/4cSDmH13RZBHV9at7) und wir werden versuchen zu helfen! Wir verfeinern Einfach.PGPT durch dein Feedback.

## 🎞️ Überblick

### Motivation hinter Einfach.PGPT
Generative KI ist ein Game-Changer für unsere Gesellschaft, aber die Einführung in Unternehmen aller Größen und datensensiblen Bereichen wie Gesundheitswesen oder Recht ist durch eine klare Sorge begrenzt: **Privatsphäre**. Nicht sicherstellen zu können, dass deine Daten vollständig unter deiner Kontrolle sind, wenn du KI-Tools von Drittanbietern verwendest, ist ein Risiko, das diese Branchen nicht eingehen können.

### Urversion
Die erste Version von Einfach.PGPT wurde im Mai 2023 als neuartiger Ansatz eingeführt, um die Datenschutzbedenken anzugehen, indem LLMs auf eine vollständig offline Weise verwendet wurden. Dies wurde durch die Nutzung bestehender Technologien erreicht, die von der florierenden Open-Source-KI-Gemeinschaft entwickelt wurden: [LangChain](https://github.com/hwchase17/langchain), [LlamaIndex](https://www.llamaindex.ai/), [GPT4All](https://github.com/nomic-ai/gpt4all), [LlamaCpp](https://github.com/ggerganov/llama.cpp), [Chroma](https://www.trychroma.com/) und [SentenceTransformers](https://www.sbert.net/).

Diese Version, die schnell zu einem Go-to-Projekt für datenschutzsensible Setups wurde und als Grundlage für Tausende von lokal fokussierten generativen KI-Projekten diente, war der Grundstein dessen, was Einfach.PGPT heutzutage wird; also eine einfachere und lehrreichere Implementierung, um die grundlegenden Konzepte zu verstehen, die erforderlich sind, um ein vollständig lokales - und daher privates - ChatGPT-ähnliches Tool zu erstellen.

Wenn du weiterhin damit experimentieren möchtest, haben wir es im [primordial branch](https://github.com/imartinez/privateGPT/tree/primordial) des Projekts gespeichert.

> Es wird dringend empfohlen, eine saubere Klonung und Installation dieser neuen Version von Einfach.PGPT durchzuführen, wenn du von der vorherigen, urzeitlichen Version kommst.

### Gegenwart und Zukunft von Einfach.PGPT
Einfach.PGPT entwickelt sich nun zu einem Gateway für generative KI-Modelle und Grundbausteine, einschließlich Vervollständigungen, Dokumenteneinlesung, RAG-Pipelines und anderen Low-Level-Bausteinen. Wir möchten es jedem Entwickler erleichtern, KI-Anwendungen und -Erfahrungen zu erstellen, sowie eine geeignete umfangreiche Architektur für die Community bereitstellen, um weiter beizutragen.

Bleibe auf dem Laufenden mit unseren [Veröffentlichungen](https://github.com/imartinez/privateGPT/releases), um alle neuen Funktionen und Änderungen zu sehen.

## 📄 Dokumentation
Die vollständige Dokumentation zu Installation, Abhängigkeiten, Konfiguration, Serverbetrieb, Bereitstellungsoptionen, Einlesen lokaler Dokumente, API-Details und UI-Funktionen findest du hier: https://docs.privategpt.dev/

## 🧩 Architektur
Konzeptionell ist Einfach.PGPT eine API, die eine RAG-Pipeline umschließt und deren Grundbausteine freilegt.
* Die API wurde mit [FastAPI](https://fastapi.tiangolo.com/) erstellt und folgt dem [API-Schema von OpenAI](https://platform.openai.com/docs/api-reference).
* Die RAG-Pipeline basiert auf [LlamaIndex](https://www.llamaindex.ai/).

Das Design von Einfach.PGPT ermöglicht es, sowohl die API als auch die RAG-Implementierung leicht zu erweitern und anzupassen. Einige wichtige architektonische Entscheidungen sind:
* Dependency Injection, Entkopplung der verschiedenen Komponenten und Schichten.
* Verwendung von LlamaIndex-Abstraktionen wie `LLM`, `BaseEmbedding` oder `VectorStore`, was es sofort ermöglicht, die tatsächlichen Implementierungen dieser Abstraktionen zu ändern.
* Einfachheit, so wenige Schichten und neue Abstraktionen wie möglich hinzuzufügen.
* Einsatzbereit, Bereitstellung einer vollständigen Implementierung der API und RAG-Pipeline.

Hauptbausteine:
* APIs sind in `private_gpt:server:<api>` definiert. Jedes Paket enthält eine `<api>_router.py` (FastAPI-Schicht) und eine `<api>_service.py` (die Service-Implementierung). Jeder *Service* verwendet LlamaIndex-Basisabstraktionen anstelle von spezifischen Implementierungen, wodurch die tatsächliche Implementierung von ihrer Nutzung entkoppelt wird.
* Komponenten befinden sich in `private_gpt:components:<component>`. Jede *Komponente* ist dafür verantwortlich, tatsächliche Implementierungen für die Basisabstraktionen bereitzustellen, die in den Services verwendet werden - zum Beispiel ist `LLMComponent` dafür verantwortlich, eine tatsächliche Implementierung eines `LLM` bereitzustellen (zum Beispiel `LlamaCPP` oder `OpenAI`).

## 💡 Mitwirken
Beiträge sind willkommen! Um die Codequalität zu gewährleisten, haben wir mehrere Format- und Typisierungsprüfungen aktiviert, führe einfach `make check` vor dem Commit durch, um sicherzustellen, dass dein Code in Ordnung ist. Denke daran, deinen Code zu testen! Du findest einen Testordner mit Hilfsmitteln, und du kannst Tests mit dem Befehl `make test` ausführen.

Interessiert daran, zu Einfach.PGPT beizutragen? Wir haben die folgenden Herausforderungen vor uns, falls du helfen möchtest:

### Verbesserungen
- Bessere RAG-Pipeline-Implementierung (Verbesserungen sowohl in den Indexierungs- als auch in den Abfragephasen)
- Code-Dokumentation
- Exposition von Ausführungsparametern wie top_p, Temperatur, max_tokens... in Vervollständigungen und Chat-Vervollständigungen
- Exposition der Chunk-Größe in der Ingest-API
- Implementierung von Update und Delete-Dokument in der Ingest-API
- Hinzufügen von Informationen über den Tokenverbrauch in jeder Antwort
- Hinzufügen zu den Vervollständigungs-APIs (Chat und Vervollständigung) der Kontextdokumente, die zur Beantwortung der Frage verwendet wurden
- Im "Modell"-Feld den tatsächlichen Namen des LLM- oder Einbettungsmodells zurückgeben

### Funktionen
- Implementierung eines Concurrency-Locks, um Fehler zu vermeiden, wenn es mehrere Aufrufe des lokalen LlamaCPP-Modells gibt
- API-Schlüssel-basierte Anforderungskontrolle zur API
- Unterstützung für Sagemaker
- Unterstützung von Funktionsaufrufen
- Hinzufügen von md5, um bereits eingelesene Dateien zu überprüfen
- Auswahl eines Dokuments zur Abfrage in der UI
- Bessere Beobachtbarkeit der RAG-Pipeline

### Projektinfrastruktur
- Verpackte Version als lokale Desktop-App (Windows-Exe, Mac-App, Linux-App)
- Dockerisierung der Anwendung für Plattformen außerhalb von Linux (Docker Desktop für Mac und Windows)
- Dokumentation, wie man auf AWS, GCP und Azure bereitstellt.

##
Um eine korrigierte Version der README und eine Konfiguration für das Einfach.GPT-Projekt auf Vercel mit Node und Python zu erstellen, habe ich das Repository [Einfach.GPT](https://github.com/Einfachalf/Einfach.GPT) überprüft. Basierend auf den verfügbaren Dateien und Informationen, hier ist ein Vorschlag für die README im Markdown-Format:

---

# Einfach.GPT – Dein persönliches ChatGPT auf deinem Computer
**14. November 2023**

## Generative KI
### Einführung:
Im Bereich der Künstlichen Intelligenz (KI), wo Datenschutz von größter Bedeutung ist, erweist sich Einfach.GPT als bahnbrechend. Dieses produktionsbereite KI-Projekt ermöglicht eine nahtlose Dokumenteninteraktion mit Large Language Models (LLMs) ohne Internetverbindung. Sein Engagement für den Datenschutz ist beispiellos – jede Interaktion mit deinen Dokumenten findet ausschließlich in deiner Ausführungsumgebung statt, was 100% Datensicherheit gewährleistet.

### Verständnis von Einfach.GPT:
Im Kern bietet Einfach.GPT eine API, die die wesentlichen Bausteine für die Erstellung privater, kontextbewusster KI-Anwendungen bereitstellt. Diese API unterstützt sowohl normale als auch Streaming-Antworten und ist vielseitig für verschiedene Anwendungen einsetzbar.

#### High-Level API: Navigation in der RAG-Pipeline:
Die API ist intelligent in zwei Hauptblöcke strukturiert, beginnend mit der High-Level-API. Diese Abstraktionsschicht vereinfacht die Komplexität der Retrieval Augmented Generation (RAG)-Pipeline. Wichtige Funktionen umfassen:

1. Dokumenteneinlesung:
   - Nahtlose Handhabung von Dokumenten-Parsing, -Aufteilung, Metadatenextraktion und Erzeugung von Einbettungen intern.
   - Effiziente Speichermechanismen für optimales Dokumentenmanagement.

2. Chat & Vervollständigungen:
   - Abstraktion von Kontextabruf, Prompt-Engineering und Antwortgenerierung unter Verwendung von Informationen aus eingelesenen Dokumenten.
   - Ermöglicht Benutzern, mit ihren Dokumenten durch natürliche Sprachanfragen zu interagieren.

#### Low-Level API: Empowerment für fortgeschrittene Benutzer:
Für Benutzer, die mehr Kontrolle und Anpassung suchen, bietet die Low-Level-API die Flexibilität, komplexe Pipelines zu implementieren. Wichtige Funktionen umfassen:

1. Erzeugung von Einbettungen:
   - Angepasste Einbettungserzeugung basierend auf spezifischen Textteilen, ermöglicht maßgeschneiderte KI-Interaktionen.

2. Abruf kontextueller Textblöcke:
   - Bei einer Anfrage werden die relevantesten Textblöcke aus den eingelesenen Dokumenten abgerufen.
   - Fortgeschrittene Benutzer können Anfragen für präzisere Informationsabrufe feinabstimmen.

### Umfassendes Toolset: Über die API hinaus:
Einfach.GPT bietet nicht nur eine robuste API, sondern auch ein umfassendes Toolset, einschließlich:

1. Gradio UI Client:
   - Ein benutzerfreundlicher Gradio UI-Client erleichtert das Testen der API und sorgt für eine reibungslose Benutzererfahrung.

2. Utility-Skripte:
   - Eine Reihe nützlicher Tools, wie ein Bulk-Model-Download-Skript, Ingestion-Skript und ein Dokumentenordner-Watcher, vereinfachen Arbeitsabläufe.

### Installation von Einfach.GPT auf deinem lokalen System: Eine Schritt-für-Schritt-Anleitung

#### Schritt 1: Klonen des Repositories
```bash
# Klonen des Einfach.GPT-Repositorys
$ git clone https://github.com/Einfachalf/Einfach.GPT
$ cd Einfach.GPT
```
Dieser Schritt beinhaltet das Herunterladen des Einfach.GPT-Codebasis vom GitHub-Repository und das Navigieren zum Projektverzeichnis.

#### Schritt 2: Installation von Python 3.11
```bash
# Erstellen einer virtuellen Umgebung mit Python 3.11
$ conda create -n Einfach.GPT python=3.11
$ conda activate Einfach.GPT
```
Dieser Schritt richtet eine virtuelle Umgebung namens `Einfach.GPT` mit Python 3.11 ein und aktiviert sie für die nachfolgenden Installationen.

#### Schritt 3: Installation von Abhängigkeiten
```bash
# Installation von Poetry und Projekt-Abhängigkeiten
$ brew install poetry
$ poetry install --with ui,local
```
Hier wird Poetry als Abhängigkeitsmanager installiert und anschließend verwendet, um die erforderlichen Projekt-Abhängigkeiten zu installieren.

#### Schritt 4: Herunterladen von Einbettungs- und LLM-Modellen
```bash
# Herunterladen von Einbettungs- und Sprachmodellen
$ poetry run python scripts/setup
```
Dieser Befehl lädt die notwendigen Modelle herunter, was bis zu 4 GB Speicherplatz benötigen kann.

#### Schritt 5: GPU-Aktivierung (Für Mac mit Metal GPU)
```bash
# Aktivierung der Metal GPU-Unterstützung
$ CMAKE_ARGS="-DLLAMA_METAL=on" pip install --force-reinstall --no-cache-dir llama-cpp-python
```
Dieser Schritt ist spezifisch für Mac mit Metal GPU. Es installiert die notwendigen Komponenten für die Metal GPU-Unterstützung.

#### Schritt 6: Starten des lokalen Servers
```bash
# Starten des lokalen Servers
$ PGPT_PROFILES=local make run
```
Dieser Befehl startet den Einfach.GPT-Lokalserver. Wenn du einen Mac mit Metal GPU verwendest, solltest du ein Log sehen, das die GPU-Nutzung anzeigt.

#### Schritt 7: Navigieren zur UI
Öffne deinen Webbrowser und gehe zu http://localhost:8001/, um auf die Einfach.GPT-UI zuzugreifen.

Diese Schritte helfen dir, das Retrieval-augmented generation (RAG)-Framework mit Einfach.GPT lokal auf einem Mac einzurichten, einschließlich der Interaktion mit Dokumenten in verschiedenen Formaten.

Wende das aus diesem Artikel gewonnene Wissen in deinen Projekten an und lass uns

 von deinen Erfahrungen wissen.

Wir schätzen dein Feedback. Fühle dich frei, deine Gedanken, Fragen oder Vorschläge im Kommentarbereich unten zu teilen.

---

### Konfiguration für Vercel mit Node und Python

Um Einfach.GPT auf Vercel zu hosten, benötigst du eine `vercel.json` Konfigurationsdatei, die angibt, wie Vercel das Projekt behandeln soll. Hier ist ein Beispiel für eine solche Konfiguration:

```json
{
  "version": 2,
  "builds": [
    {
      "src": "package.json",
      "use": "@vercel/node"
    },
    {
      "src": "pyproject.toml",
      "use": "@vercel/python"
    }
  ],
  "routes": [
    {
      "src": "/(.*)",
      "dest": "/"
    }
  ]
}
```

Diese Konfiguration gibt an, dass Vercel sowohl Node.js- als auch Python-Abhängigkeiten für das Projekt installieren und bauen soll. Die `routes`-Sektion leitet alle Anfragen an die Hauptanwendung weiter.

Stelle sicher, dass du alle erforderlichen Abhängigkeiten und Skripte in deinen `package.json` und `pyproject.toml` Dateien korrekt definiert hast, damit Vercel das Projekt korrekt bauen und ausführen kann.
