# Knowledge Base Docs

## Beschreibung / Description

Dieses Repository dient zur Speicherung aufbereiteter technischer Dokumentationen (Markdown, Keywords, Metadaten) von verschiedenen Online-Quellen wie OpenAI, Twilio und anderen. Die Daten werden durch ein Python-Skript verarbeitet und sind für die Verwendung in Wissensdatenbanken, zur Generierung von Embeddings und für Retrieval-Augmented Generation (RAG)-Systeme vorbereitet.

This repository stores processed technical documentation (Markdown, keywords, metadata) from various online sources like OpenAI, Twilio, etc. The data is processed by a Python script and prepared for use in knowledge bases, embedding generation, and Retrieval-Augmented Generation (RAG) systems.

## Features

* Ruft Webinhalte von angegebenen URLs ab.
* Extrahiert den Hauptinhalt von Dokumentationsseiten mithilfe von CSS-Selektoren.
* Konvertiert den extrahierten HTML-Inhalt in sauberes Markdown.
* Extrahiert grundlegende Keywords aus dem Textinhalt.
* Generiert Metadaten (Original-URL, Titel, Quelle, Abrufzeitstempel, Keywords).
* Speichert die Ausgabe als strukturierte JSON-Dateien.
* Organisiert die Ausgabedateien in Ordnern basierend auf der Quelldomain.

## Ordnerstruktur / Folder Structure

knowledge-base-docs/
├── .gitignore             # Ignored files for Git
├── README.md              # This file
├── requirements.txt       # Python dependencies
│
├── config/                # (Optional) Configuration files (e.g., sources.json)
│
├── src/                   # Source code of the processing tool
│   └── doc_processor.py   # The main Python script
│
└── knowledge_base/        # Root folder for the generated knowledge base files
├── openai/            # Subfolder per source (automatically generated)
│   └── some_openai_doc.json
│   └── ...
├── twilio/
│   └── another_doc_from_twilio.json
│   └── ...
└── another_source/
└── ...


## Voraussetzungen / Prerequisites

* Git
* Python 3.8+
* pip (Python package installer)

## Installation & Setup

1.  **Repository klonen / Clone the repository:**
    ```bash
    git clone <your-repository-url>
    cd knowledge-base-docs
    ```

2.  **Virtuelle Umgebung erstellen (empfohlen) / Create virtual environment (recommended):**
    ```bash
    python -m venv venv
    # Linux/macOS:
    source venv/bin/activate
    # Windows:
    .\venv\Scripts\activate
    ```

3.  **Abhängigkeiten installieren / Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

4.  **NLTK-Daten herunterladen / Download NLTK data:**
    Das Skript verwendet NLTK zur Keyword-Extraktion. Führe einmalig den folgenden Python-Code aus, um die benötigten Datenpakete herunterzuladen:
    ```bash
    python -c "import nltk; nltk.download('punkt'); nltk.download('stopwords')"
    ```

## Verwendung / Usage

Das Kernskript `doc_processor.py` wird über die Kommandozeile ausgeführt.

**Grundlegender Befehl / Basic command:**

```bash
python src/doc_processor.py --url <URL_TO_PROCESS> --output-dir knowledge_base [OPTIONS]
Wichtige Argumente / Key Arguments:

--url URL: (Erforderlich) Die URL der Dokumentationsseite, die verarbeitet werden soll.
--output-dir PATH: (Erforderlich) Das Basisverzeichnis, in dem die Ausgabedateien gespeichert werden sollen. Verwende knowledge_base, um die Daten innerhalb dieses Repos zu speichern.
--selector CSS_SELECTOR: (Optional) Der CSS-Selektor, der verwendet wird, um den Hauptinhaltsbereich der Seite zu identifizieren. Dies ist oft der wichtigste und am schwierigsten zu bestimmende Parameter. Verwende die Entwicklertools deines Browsers (Rechtsklick -> Untersuchen), um den richtigen Selektor zu finden (z.B. main, article, #content, .main-content). Wenn nicht angegeben, versucht das Skript Standardselektoren (article, main, etc.).
--source-name NAME: (Optional) Überschreibt den automatisch aus der Domain abgeleiteten Namen der Quelle (z.B. "OpenAI", "Twilio").
--num-keywords NUM: (Optional) Die maximale Anzahl der zu extrahierenden Keywords (Standard: 15).
Beispiel / Example:

Verarbeiten der Twilio SMS Quickstart-Seite und Speichern der Ausgabe im knowledge_base-Ordner, wobei div.content-column als Hauptinhaltsselektor verwendet wird:

Bash

# Stelle sicher, dass du im Hauptverzeichnis des Repos (knowledge-base-docs/) bist
python src/doc_processor.py \
    --url "[https://www.twilio.com/docs/sms/quickstart/python](https://www.twilio.com/docs/sms/quickstart/python)" \
    --output-dir "knowledge_base" \
    --selector "div.content-column" \
    --source-name "Twilio"
Ausgabe / Output
Für jede verarbeitete URL erstellt das Skript eine JSON-Datei in der folgenden Struktur:

knowledge_base/<source_name>/<sanitized_filename>.json

<source_name> wird vom Argument --source-name oder automatisch von der URL-Domain abgeleitet.
<sanitized_filename> wird aus dem Pfad der URL generiert und für Dateisysteme bereinigt.
Jede JSON-Datei enthält zwei Hauptschlüssel:

metadata: Ein Objekt mit Informationen über die Quelle und den Verarbeitungsvorgang.
markdown_content: Eine Zeichenkette, die den extrahierten und konvertierten Inhalt im Markdown-Format enthält.
Beispiel JSON-Struktur:

JSON

{
  "metadata": {
    "source_url": "[https://www.example.com/docs/topic/page](https://www.google.com/search?q=https://www.example.com/docs/topic/page)",
    "title": "Example Documentation Page Title",
    "source_domain": "[www.example.com](https://www.example.com)",
    "source_name": "ExampleSource",
    "fetch_timestamp": "2025-03-31T00:40:00+00:00",
    "keywords": ["example", "topic", "page", "docs", "api"],
    "summary": "Optional short summary..."
  },
  "markdown_content": "# Example Documentation Page Title\n\nThis is the main content converted to Markdown...\n\n```python\nprint('Hello, world!')\n```\n\n## Sub-heading\n\nMore details here..."
}
Konfiguration (Optional) / Configuration (Optional)
Der Ordner config/ kann verwendet werden, um Konfigurationen auszulagern, z.B. eine sources.json-Datei, die Listen von URLs, bevorzugte Selektoren pro Domain oder andere Einstellungen enthält. Das Skript müsste entsprechend angepasst werden, um diese Konfigurationen zu lesen.

Lizenz / License
Dieses Projekt steht unter der MIT-Lizenz. Siehe die LICENSE-Datei (falls vorhanden) für Details. / This project is licensed under the MIT License. See the LICENSE file (if available) for details.

Beitragende / Contributing
Beiträge sind willkommen! Bitte öffne ein Issue, um Fehler zu melden oder Funktionen vorzuschlagen, oder erstelle einen Pull Request. / Contributions are welcome! Please open an issue to report bugs or suggest features, or submit a pull request.