# Zasady współpracy

Ten dokument opisuje, jak pracujemy nad projektem **Biuro Zaginionych Skarpetek**: jak tworzymy gałęzie, piszemy commity i zgłaszamy Pull Requesty.

## Ogólne zasady

1. Każda osoba odpowiada za **jedną sekcję** strony w pliku `index.html` (`#sekcja-1` … `#sekcja-4`).
2. Pracujesz tylko w obrębie **swojej sekcji**. Nie zmieniasz sekcji innych osób bez ich zgody.
3. Zmiany we wspólnych częściach (`<head>`, zmienne w `style.css`, klasa `.kontener`) uzgadniamy z całym zespołem i wprowadzamy osobnym Pull Requestem.
4. W CSS każdy selektor dotyczący Twojej sekcji zaczynasz od jej identyfikatora, np. `#sekcja-2 h2 { ... }`. Dzięki temu nie zmienisz wyglądu innych sekcji.
5. **Nikt nie wysyła zmian bezpośrednio do głównego repozytorium.** Każdy pracuje na swoim forku, a zmiany trafiają do głównego repo przez Pull Request.
6. Każde zadanie ma swoją kartę w Trello, a jej status aktualizujemy na bieżąco.

## Forki

Pracujemy w modelu **fork + Pull Request**. Główne repozytorium (tzw. **upstream**) należy do jednej osoby z zespołu. Pozostali nie wysyłają do niego zmian bezpośrednio, tylko pracują na własnej kopii (forku) i zgłaszają zmiany Pull Requestem.

### Przygotowanie (jednorazowo)

1. Wejdź na stronę głównego repozytorium na GitHubie i kliknij **Fork**. Na Twoim koncie pojawi się kopia repozytorium.
2. Sklonuj **swój fork**:
   ```bash
   git clone https://github.com/<twoj-login>/bzs-strona.git
   cd bzs-strona
   ```
3. Dodaj główne repozytorium jako zdalne `upstream`:
   ```bash
   git remote add upstream https://github.com/<login-wlasciciela>/bzs-strona.git
   git remote -v
   ```
   Powinny być widoczne dwa zdalne repozytoria: `origin` (Twój fork) i `upstream` (główne repo).

### Aktualizacja forka

Przed rozpoczęciem każdej nowej pracy pobierz najnowsze zmiany z głównego repozytorium:

```bash
git checkout main
git fetch upstream
git merge upstream/main
git push origin main
```

Możesz też użyć przycisku **Sync fork** na stronie swojego forka na GitHubie, a potem zrobić `git pull` lokalnie.

## Gałęzie

Gałąź `main` zawsze zawiera działającą wersję strony. Nawet we własnym forku **nie pracujemy bezpośrednio na `main`**: dla każdego zadania tworzymy nową gałąź od zaktualizowanego `main`:

```bash
git checkout main
git checkout -b feature/sekcja-1
```

### Nazewnictwo gałęzi

| Typ        | Kiedy                          | Przykład                |
| ---------- | ------------------------------ | ----------------------- |
| `feature/` | nowa funkcjonalność lub sekcja | `feature/sekcja-2`      |
| `fix/`     | poprawka błędu                 | `fix/menu-na-telefonie` |
| `docs/`    | zmiany w dokumentacji          | `docs/readme`           |
| `style/`   | zmiany we wspólnych stylach    | `style/kolory`          |

Nazwy piszemy małymi literami, bez polskich znaków, a słowa łączymy myślnikiem.

## Commity

- Commitujemy **często i małymi kawałkami**: jeden commit to jedna logiczna zmiana.
- Wiadomość piszemy po polsku, w trybie rozkazującym, zaczynając od numeru sekcji lub obszaru zmian:

```
Sekcja 1: dodaj logo i menu
Sekcja 2: popraw odstępy między kartami
Style: dodaj kolor niebieski do zmiennych
Docs: uzupełnij skład zespołu w README
```

- Wiadomość ma mówić, **co** zmienia commit. Unikamy opisów typu `poprawki`, `zmiany` czy `asdf`.
- Nie commitujemy plików, które nie należą do projektu (np. `.DS_Store`, ustawień edytora).

## Pull Requesty

1. Wyślij swoją gałąź do **swojego forka**:
   ```bash
   git push -u origin feature/sekcja-1
   ```
2. Na GitHubie otwórz Pull Request z gałęzi w swoim forku (`<twoj-login>/bzs-strona`, `feature/sekcja-1`) do gałęzi `main` w **głównym repozytorium** (`<login-wlasciciela>/bzs-strona`). GitHub zwykle sam proponuje to przyciskiem **Compare & pull request**.
3. Uzupełnij opis PR-a:
   - **Tytuł:** krótko, co zawiera, np. `Sekcja 1: nagłówek i hero`.
   - **Opis:** co zostało zrobione i jak to sprawdzić.
   - **Trello:** link do karty zadania.
   - **Zrzut ekranu:** jeśli zmiana jest widoczna na stronie.
4. Przypisz **co najmniej jedną osobę** z zespołu do review.
5. PR można zmergować dopiero po **akceptacji przez co najmniej jedną osobę**, ale nie przez autora.
6. Po zmergowaniu usuń gałąź w swoim forku, zaktualizuj fork (zobacz „Aktualizacja forka”) i przesuń kartę w Trello do kolumny „Zrobione”.

### Code review

Osoba robiąca review:

- pobiera zmiany z PR-a i otwiera `index.html` w przeglądarce, żeby sprawdzić, czy strona działa. Najprościej przez GitHub CLI: `gh pr checkout <numer-PR>`, albo:
  ```bash
  git fetch upstream pull/<numer-PR>/head:review-pr
  git checkout review-pr
  ```
- sprawdza, czy zmiany dotyczą tylko właściwej sekcji,
- zostawia konkretne i życzliwe komentarze,
- zatwierdza PR (**Approve**) albo prosi o poprawki (**Request changes**).

## Konflikty

Wszyscy pracujemy w jednym pliku `index.html`, więc konflikty przy mergowaniu są normalne. GitHub pokaże w PR-ze komunikat „This branch has conflicts”. Wtedy zaktualizuj swoją gałąź o najnowszy `main` z głównego repozytorium:

```bash
git checkout feature/sekcja-1
git fetch upstream
git merge upstream/main
```

Jeśli Git zgłosi konflikt:

1. Otwórz plik z konfliktem i znajdź znaczniki `<<<<<<<`, `=======` i `>>>>>>>`.
2. Zostaw zmiany **obu** stron: swoją sekcję i sekcje innych osób.
3. Usuń znaczniki konfliktu i sprawdź, czy strona działa w przeglądarce.
4. Zatwierdź rozwiązanie:
   ```bash
   git add index.html
   git commit -m "Rozwiąż konflikt z main"
   git push origin feature/sekcja-1
   ```
   PR zaktualizuje się automatycznie.

W razie wątpliwości rozwiązujcie konflikt razem z autorem drugiej zmiany.
