# AI pisało, Ty łamiesz

> Program Bug Bounty dla [twojakategoria.pl](https://www.twojakategoria.pl) — polskiego portalu edukacyjnego zbudowanego w całości przez AI (vibe coding). Pula nagród: **2 000 zł**.

Zbudowałem tę aplikację razem z AI. Obsługuje realne dane użytkowników i płatności. Teraz płacę Ci za jej złamanie.

---

## Środowisko testowe

**Testuj wyłącznie tutaj:**
```
https://twojakategoria-git-staging-kamilgpuks-projects.vercel.app
```

Produkcja (`twojakategoria.pl`) jest poza zakresem. Testy na produkcji = dyskwalifikacja.

---

## Pule nagród

Po zakończeniu programu (30 dni) pula każdego poziomu dzielona jest **równo** między wszystkich zakwalifikowanych badaczy w danym tierze.

| Poziom | Pula | Maks. osób | Zasada |
|--------|------|------------|--------|
| 🔴 Krytyczny | 1 000 zł | 5 | first come, first served |
| 🟠 Wysoki | 500 zł | 5 | first come, first served |
| 🟡 Średni | 300 zł | 5 | first come, first served |
| 🟢 Niski | 200 zł | 5 | first come, first served |

**Przykład:** 4 osoby zgłoszą krytyczne bugi → każda dostaje 250 zł.

**Niezagospodarowane pule** (np. nikt nie znajdzie krytycznego buga) przekazuję w całości na [Fundacja Daj Herbatę](https://www.dajherbate.pl).

**Duplikaty:** jeśli dwie osoby zgłoszą ten sam bug, liczy się tylko pierwsze zgłoszenie.

---

## Co jest w zakresie

### Aplikacja webowa (staging)
- Wszystkie strony publiczne i landing page
- Kreator dokumentów (6 kroków) — pola formularza, walidacja, sesja
- Strona podglądu dokumentu
- Strona sukcesu płatności i pobierania PDF

### Endpointy API
| Endpoint | Co testować |
|----------|-------------|
| `/api/payment/*` | Inicjacja płatności, callback, weryfikacja podpisu |
| `/api/download/[token]` | Autoryzacja pobierania PDF |
| `/api/pdf/*` | Generowanie i status PDF |
| `/api/lead-magnet` | Zapis checklisty — walidacja, rate limiting |
| `/api/track/*` | Eventy analityczne — injection, spoofing |
| `/api/unsubscribe` | Wypisanie — autoryzacja |

### Obszary szczególnego zainteresowania
- **Obejście płatności** — dostęp do PDF bez zapłaty *(priorytet krytyczny)*
- **IDOR** — dostęp do cudzego PDF lub sesji *(priorytet krytyczny)*
- **Manipulacja sesją** — dane w Redis, tokeny pobierania
- **XSS** — w polach formularza trafiających do wygenerowanego PDF
- **CSRF** — na akcjach płatności i zapisu
- **Bypass rate limitera** — lead magnet, inicjacja płatności
- **Injection** — w polach osobowych i medycznych
- **Wycieki danych** — przez odpowiedzi API, nagłówki, błędy

---

## Co jest poza zakresem

### Infrastruktura zewnętrzna
- PayNow / mBank — systemy płatności (nie nasza infrastruktura)
- Vercel, Cloudflare, Upstash, Resend, Discord — dostawcy usług
- Meta Pixel / CAPI

### Wykluczone techniki
- **DoS / DDoS** — jakiekolwiek próby przeciążenia serwisu
- **Automatyczne skany bez writeupa** — dumpy z Burpa/Nikto bez analizy = odrzucenie
- **Social engineering, phishing, vishing**
- **Ataki wymagające dostępu fizycznego** do urządzenia ofiary
- **Self-XSS** — atak wymagający własnych działań w przeglądarce
- **Testy na środowisku produkcyjnym** (`twojakategoria.pl`)

### Niski priorytet (mogą nie kwalifikować się do nagrody)
- Brakujące nagłówki HTTP bez możliwego wektora ataku
- Clickjacking na stronach bez wrażliwych akcji
- Konfiguracja SSL/TLS (zarządza Cloudflare)
- Problemy wyłącznie w przeglądarkach starszych niż 2 lata

---

## Jak zgłosić buga

Używamy **GitHub Private Vulnerability Reporting** — Twoje zgłoszenie jest widoczne tylko dla organizatora, nie publicznie.

👉 **[Zgłoś podatność](https://github.com/kamilgpuk/twojakategoria-security/security/advisories/new)**

### Co powinno zawierać zgłoszenie
1. **Tytuł** — krótki opis podatności
2. **Opis** — czego dotyczy, jaki jest potencjalny wpływ
3. **Kroki reprodukcji** — dokładna sekwencja do odtworzenia
4. **Dowód** — screenshot, nagranie, PoC (proof of concept)
5. **Proponowany poziom** — Krytyczny / Wysoki / Średni / Niski

Writeup bez PoC = zgłoszenie nierozpatrzone.

---

## Kryteria poziomów

| Poziom | Kryteria |
|--------|----------|
| 🔴 **Krytyczny** | Dostęp do cudzych danych/PDF, obejście płatności, RCE, masowy wyciek danych |
| 🟠 **Wysoki** | XSS z możliwością kradzieży sesji, CSRF na płatnościach, manipulacja sesją Redis, IDOR, istotny wyciek danych |
| 🟡 **Średni** | Bypass rate limitera, open redirect, wyciek wrażliwych danych przez API, logika biznesowa |
| 🟢 **Niski** | Drobne wycieki informacji, brakujące nagłówki z realnym wpływem, błędy walidacji |

Ostateczny poziom ustala organizator. Decyzja jest wiążąca.

---

## Zasady ogólne

- Program trwa od **31 marca** do **30 kwietnia 2026**
- Testy tylko na środowisku staging — produkcja jest zakazana
- Nie wolno uzyskiwać dostępu do danych innych użytkowników stagingu
- Nie wolno niszczyć danych ani zakłócać działania serwisu
- Każdy badacz może mieć maksymalnie **jedno miejsce per tier** (pierwsze zgłoszenie)
- Podatności znalezione po zamknięciu programu nie kwalifikują się do nagród
- Organizator zastrzega prawo do odrzucenia zgłoszeń naruszających zasady

---

## Wypłata

Po zakończeniu programu (podsumowanie wyników) kontaktuję się z każdym zakwalifikowanym badaczem przez GitHub.

Wypłata do wyboru:
- **BLIK** — po wymianie wiadomości prywatnych
- **Przelew na konto wskazanej organizacji non-profit** (NGO)

---

## O projekcie

[twojakategoria.pl](https://www.twojakategoria.pl) to polski portal edukacyjny pomagający mężczyznom w legalnej zmianie kategorii wojskowej z powodu pogorszenia stanu zdrowia. Aplikacja generuje dokumenty po stronie klienta (dane medyczne nigdy nie opuszczają przeglądarki), obsługuje płatności przez PayNow/mBank i działa zgodnie z RODO.

Zbudowana w całości przy użyciu AI (Next.js 14, TypeScript, Tailwind CSS). Kod źródłowy pozostaje prywatny.

---

*Pytania dotyczące zakresu? Otwórz [Discussion](https://github.com/kamilgpuk/twojakategoria-security/discussions) w tym repozytorium.*
