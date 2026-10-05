# Mini-projekt DS02 — jak wrzucać swoje pliki na repo

Ta instrukcja pokazuje **krok po kroku**, jak skonfigurować repozytorium raz na początku,
a potem jak wrzucać kolejne etapy projektu (README, notebooki) w miarę postępu prac.

Repo: **github.com/michalszlapa/mini_project_DS02_05_26**

---

## Zanim zaczniesz — czego potrzebujesz

- Konto na GitHub (jeśli nie masz — załóż na [github.com](https://github.com))
- Zainstalowany Git na komputerze ([git-scm.com](https://git-scm.com/downloads))
- Podstawowa znajomość terminala/PowerShell (wystarczy wklejać komendy poniżej)

---

## Krok 1 — Fork repozytorium (robisz TYLKO RAZ)

1. Wejdź na **github.com/michalszlapa/mini_project_DS02_05_26**
2. W prawym górnym rogu kliknij przycisk **Fork**
3. Zatwierdź (Create fork) — teraz masz własną kopię repo pod adresem
   `github.com/TWÓJ_LOGIN/mini_project_DS02_05_26`

To jest jednorazowa operacja — nie powtarzasz jej przy kolejnych etapach.

---

## Krok 2 — Klonowanie na komputer (robisz TYLKO RAZ)

Klonujesz **swój fork** (nie oryginalne repo Michała):

```bash
git clone https://github.com/TWÓJ_LOGIN/mini_project_DS02_05_26.git
cd mini_project_DS02_05_26
```

---

## Krok 3 — Stwórz swój folder (robisz TYLKO RAZ, przy pierwszym etapie)

Wszystkie Twoje pliki mieszkają w **jednym** folderze o nazwie:

```
projekt/literaimienia_nazwisko/
```

Przykład: Anna Kowalska → `projekt/a_kowalska/`

```bash
mkdir -p projekt/literaimienia_nazwisko
```

Skopiuj do niego pierwszy plik (np. `README.md` z opisem datasetu) i przejdź do Kroku 4.

---

## Krok 4 — Pierwszy commit i pierwszy Pull Request

To jedyny moment, w którym otwierasz **nowy** Pull Request — przy pierwszym etapie.

```bash
git add .
git commit -m "etap 1: opis datasetu - Imię Nazwisko"
git push
```

Potem na GitHubie:

1. Wejdź na swój fork (`github.com/TWÓJ_LOGIN/mini_project_DS02_05_26`)
2. Zobaczysz żółty pasek **"Compare & pull request"** — kliknij go
3. Sprawdź, że kierunek jest poprawny: `base: michalszlapa/mini_project_DS02_05_26`
   ← `head: TWÓJ_LOGIN/mini_project_DS02_05_26`
4. Tytuł PR: **Imię Nazwisko — mini-projekt**
5. Kliknij **Create pull request**

Gotowe — PR jest otwarty i widoczny dla prowadzącego.

---

## Krok 5 — Kolejne etapy (2, 3, 4, 5...) — TO SIĘ POWTARZA

To jest najważniejsza część tej instrukcji: **NIE zakładasz nowego forka, NIE klonujesz
drugi raz, NIE otwierasz nowego PR-a.** Kolejny etap to po prostu nowy plik w tym samym
folderze + nowy commit + push — Twój istniejący PR **zaktualizuje się sam**.

```bash
# 1. Dodaj nowy plik (np. 02_transformacja.ipynb) do swojego folderu projekt/literaimienia_nazwisko/

# 2. W terminalu, w folderze repo:
git add .
git commit -m "etap 2: transformacja danych - Imię Nazwisko"
git push
```

Po `git push` wróć na swój Pull Request na GitHubie — zobaczysz, że nowy commit
pojawił się automatycznie na liście. Nic więcej nie musisz robić.

**Powtarzaj ten sam schemat (`git add .` → `git commit` → `git push`) dla każdego
kolejnego etapu**, zmieniając tylko treść commit message.

---

## Ściągawka — cała procedura w skrócie

| Kiedy | Co robisz |
|---|---|
| Raz, na starcie | Fork → Clone → stwórz `projekt/literaimienia_nazwisko/` |
| Etap 1 | `git add .` → `git commit` → `git push` → **otwórz PR** |
| Etap 2, 3, 4, 5... | `git add .` → `git commit` → `git push` → *(PR aktualizuje się sam)* |

---

## Najczęstsze problemy

**"Nic się nie stało po `git push`"** — sprawdź, czy jesteś w folderze repo
(`cd mini_project_DS02_05_26`) i czy plik faktycznie zapisałeś w
`projekt/literaimienia_nazwisko/` przed `git add .`.

**"Push się nie udał, błąd o rozbieżnych branchach"** — ktoś (albo Ty na innym
komputerze) zmienił coś w międzyczasie. Napraw przez:
```bash
git pull --rebase
git push
```

**"Nie wiem, czy mój PR jest widoczny"** — wejdź na
`github.com/michalszlapa/mini_project_DS02_05_26/pulls` — powinieneś zobaczyć swój
PR na liście otwartych.

**"Pomyliłem nazwę folderu"** — popraw przez `git mv stary_folder projekt/poprawna_nazwa`,
potem `git add .` → `git commit -m "poprawka nazwy folderu"` → `git push`.

---

## Struktura Twojego folderu (przypomnienie)

```
projekt/literaimienia_nazwisko/
├── README.md                    (etap 1 — opis datasetu)
├── 01_czyszczenie.ipynb         (etap 2)
├── 02_transformacja.ipynb       (etap 3)
├── 03_eda.ipynb                 (etap 4)
└── 04_model.ipynb               (etap 5)
```

Jeden folder, jeden PR, kolejne commity — bez wyjątków i bez nowych folderów po drodze.
