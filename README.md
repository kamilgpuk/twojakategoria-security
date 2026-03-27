# Vibe Coding → Bug Bounty → Kasa dla NGO

> Program Bug Bounty dla [twojakategoria.pl](https://www.twojakategoria.pl) — portalu edukacyjnego który zbudowałem w całości przez AI (vibe coding). Pula nagród: **2 000 zł przekazane na NGO**.

Zbudowałem tę aplikację razem z AI, nie będąc programistą. Z jednej strony bardzo uskrzydlające doświadczenie - z drugiej, mając za sobą 10 lat w IT wiem, że nic nie wiem. Apka obsługuje realne dane użytkowników, ma podpięte płatności - jestem ciekaw, jakig bugów i podatności nie znalazły dotychczas agenty. W ramach ciekawości, oraz w duchu *giving back*, stawiam kasę za jej złamanie — a nagrodą jest wskazanie organizacji non-profit, na którą trafi Twoja część puli.

---

## Środowisko testowe

**Testuj wyłącznie tutaj:**
```
https://twojakategoria-git-staging-kamilgpuks-projects.vercel.app
```

Produkcja (`twojakategoria.pl`) jest poza zakresem. Testy na produkcji = dyskwalifikacja. Chodzi o naukę i NGOs, nie pracę na czyjąś szkodę.

---

## Pule nagród

Po 20.04 pula każdego poziomu dzielona jest **równo** między autorów zgłoszeń w danym tierze, do limitu unikatowych zgłoszeń. Każdy zakwalifikowany autor **wskazuje NGO**, na które trafi jego część — a ja przekazuje środki bezpośrednio i publikuję moim LI.

| Poziom | Pula | Maks. osób | Zasada |
|--------|------|------------|--------|
| 🔴 Krytyczny | 1 000 zł | 5 | first come, first served |
| 🟠 Wysoki | 500 zł | 5 | first come, first served |
| 🟡 Średni | 300 zł | 5 | first come, first served |
| 🟢 Niski | 200 zł | 5 | first come, first served |

**Przykład:** 4 osoby zgłoszą krytyczne bugi → każda wskazuje NGO, na które trafi 250 zł.

**Niezagospodarowane pule** (np. nikt nie znajdzie krytycznego buga) trafiają w całości na [Fundację Daj Herbatę](https://www.dajherbate.pl).

**Duplikaty:** jeśli dwie osoby zgłoszą ten sam bug, liczy się tylko pierwsze zgłoszenie.

---

## Co jest w zakresie

### Aplikacja webowa (staging)
- Wszystkie strony publiczne i landing page
- Kreator dokumentów (6 kroków) — pola formularza, walidacja, sesja
- Strona podglądu dokumentu
- Strona sukcesu płatności i pobierania PDF
- Błędy w danych katalogowych (zły adres WCR, etc)

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
- **DoS / DDoS** — jakiekolwiek próby przeciążenia serwisu; pls don't
- **Automatyczne skany bez writeupa** — dumpy z Burpa/Nikto bez analizy = odrzucenie
- **Social engineering, phishing, vishing**
- **Testy na środowisku produkcyjnym** (`twojakategoria.pl`) - be a nice human, daj mi czas naprawić błędy po znalezieniu ich na stage.

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

---

## Kryteria poziomów

| Poziom | Kryteria |
|--------|----------|
| 🔴 **Krytyczny** | Dostęp do cudzych danych/PDF, obejście płatności, RCE, masowy wyciek danych |
| 🟠 **Wysoki** | XSS z możliwością kradzieży sesji, CSRF na płatnościach, manipulacja sesją Redis, IDOR, istotny wyciek danych |
| 🟡 **Średni** | Bypass rate limitera, open redirect, wyciek wrażliwych danych przez API, logika biznesowa |
| 🟢 **Niski** | Drobne wycieki informacji, brakujące nagłówki z realnym wpływem, błędy walidacji |

---

## Zasady ogólne

- Program trwa od **31 marca** do **20 kwietnia 2026**
- Testy tylko na środowisku staging, please
- Nie wolno niszczyć danych ani zakłócać działania serwisu produkcyjnego
- Postaram się publikować listę zgłoszonych rzeczy jak tylko będę je łatać

---

## Przekazanie nagrody

Po zakończeniu programu kontaktuję się z każdym autorem przez GitHub, wskazujecie organizację non-profit (NGO) według własnego wyboru — a ja przekazuje środki bezpośrednio na jej konto.

Niezagospodarowane pule, do wysokości sumy nagród 2000zł, trafiają na [Fundację Daj Herbatę](https://www.dajherbate.pl).

---

## O projekcie

[twojakategoria.pl](https://www.twojakategoria.pl) to portal edukacyjny pomagający mężczyznom w legalnej zmianie kategorii wojskowej z powodu pogorszenia stanu zdrowia. Aplikacja generuje dokumenty po stronie klienta (dane medyczne nigdy nie opuszczają przeglądarki), obsługuje płatności przez PayNow/mBank i działa zgodnie z RODO.

Zbudowana w całości przy użyciu AI (Next.js 14, TypeScript, Tailwind CSS). Kod źródłowy pozostaje prywatny.

---

*Pytania dotyczące zakresu? Otwórz [Discussion](https://github.com/kamilgpuk/twojakategoria-security/discussions) w tym repozytorium.*
