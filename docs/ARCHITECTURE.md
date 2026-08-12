# Architektura

Dokumentacja architektoniczna projektu.

---

## Schemat Bazy Danych (PostgreSQL + PostGIS)

Projekt bazy danych uwzględnia obsługę geodanych (PostGIS) do filtrowania ofert według lokalizacji i obszarów na mapie.

* **Interaktywny diagram ERD:** [dbdiagram.io - Schemat Bazy Danych](https://dbdiagram.io/d/69c3d600fb2db18e3b0015c8)

---

## Przepływ Danych

1. **Scraper Engine (Python):** Scrapuje pierwszą stronę portalu (co 5 minut) i pełny portal (raz na dobę), po czym publikuje wiadomość o nowej/zaktualizowanej ofercie do kolejki.
2. **Message Broker (RabbitMQ):** Przechowuje i buforuje zdarzenia przychodzące ze scraperów.
3. **Core API & Workers (.NET Core):** 
   * **API / Consumer:** Nasłuchuje na zdarzenia z kolejki, przetwarza dane, aktualizuje bazę PostgreSQL i wyzwala powiadomienia.
   * **Verification Worker:** Serwis weryfikacyjny, który sprawdza oferty nieaktualizowane od ponad 24h.
4. **Web Client (Next.js):** Prezentuje dane użytkownikowi końcowemu (SSR/SSG).
