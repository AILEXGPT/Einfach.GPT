
---

# Einfach.GPT – Dein persönliches ChatGPT auf deinem Computer


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
