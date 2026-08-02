## Instrukcja instalacji wersji testowej v2.0 (testV2)

### Ten poradnik przeprowadzi Cię przez proces ręcznej instalacji wersji testowej karty piotras-smart-button w wersji 2.0

### ⚠️ BARDZO WAŻNE (Krok 1):
**Jeśli masz obecnie zainstalowaną oficjalną wersję tej karty przez HACS, musisz ją najpierw całkowicie odinstalować w HACS (oraz upewnić się, że w zasobach Dashboardu nie zostaną stare wpisy powodujące konflikt). Posiadanie dwóch wersji jednocześnie sprawi, że karta będzie się „wysypywać” i nie uruchomi poprawnie nowej składni!**

#### 2. Pobranie plików z repozytorium
**Przejdź do katalogu [testV2](https://github.com/Piotras1/piotras-smart-button/tree/main/testV2) w repozytorium GitHub i pobierz na swój dysk lokalny następujące trzy pliki:**  
- piotras-smart-button.js
- piotras-smart-button-editor.js
- piotras-smart-button-loader.js

#### 3. Umieszczenie plików na dysku Home Assistanta
- Przenieś pobrane pliki do katalogu na swoim Home Assistantcie (poprzez menedżer plików, np. File Editor lub Samba Share):
do katalogu **config/www/piotras-smart-button/**
- Jeśli katalog **piotras-smart-button** nie istnieje w folderze www, utwórz go.

#### 4. Dodanie zasobu (Resource) w Home Assistant
**Aby Home Assistant wiedział o nowej karcie, musisz zarejestrować plik ładujący:**
- Wejdź w swoim Home Assistant do: Ustawienia -> Pulpity lovelace -> zakładka Zasoby (Resources).
- Kliknij Dodaj zasób w prawym dolnym rogu.
- W polu URL wpisz dokładnie:  **/local/piotras-smart-button/piotras-smart-button-loader.js?v=2.0**
- W polu Typ wybierz Moduł JavaScript (JavaScript Module).
- Kliknij Zapisz.

#### 5. Odświeżenie przeglądarki
- Gotowe! Teraz odśwież swoją przeglądarkę (najlepiej z wyczyszczeniem pamięci podręcznej, np. Ctrl + F5 albo zrestartuj aplikację mobilną HA), stwórz nową kartę typu custom:piotras-smart-button i ciesz się nowymi możliwościami wersji 2.0! 
