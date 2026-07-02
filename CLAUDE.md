# BMT Modern Lighting / BMteam — projekt strony

Jednostronicowa strona (landing page) dla firmy oświetleniowej. Czysty HTML + Tailwind (CDN) +
vanilla JS — **bez build stepu, bez frameworka**. Cały kod w jednym pliku `index.html`.

## Struktura plików
```
bmt-modern-lighting/
  index.html              ← cała strona (HTML+CSS+JS w jednym pliku)
  assets/
    logo-bmt.svg           ← oficjalne logo (pomarańczowy blokowy znak + "BMteam")
    products/               ← 5 zdjęć produktowych, białe tło 1400×1400px, wyciągnięte z katalogu PDF
      m2-wall-points-6w.png
      m2-line-24w.png
      legacy-4x-15w.png
      moonlight-80w.png
      m2-pendant-7w.png
```

## Marka / nazewnictwo (nierozwiązana niespójność)
- **Logo** mówi "BMteam" (bez taglinu — poprzednia wersja z "geometry of light" była błędna, klient przysłał poprawkę).
- **Treść strony** (nagłówki, opisy) używa "BMT" jako skrótu w zdaniach (np. "każdy projekt BMT").
- Ustalono: `<title>`, meta description i copyright w stopce → "BMteam". Reszta kopii zostaje przy "BMT" jako naturalny skrót. Użytkownik zaakceptował to rozwiązanie, ale nie było to pełne ujednolicenie — jeśli klient zapyta, można zaproponować pełną zamianę.

## Design system
- **Kolory** (w `tailwind.config` w `<head>`): `ink #040302` (tło), `surface #0c0a08`, `surface2 #151210`,
  `gold #d9a24b`, `amber #f2994a` (główny akcent), `ember #d6390f` (gradient CTA/badge),
  `coral #ff7a52` (dostępny tekst na ciemnym tle zamiast ember), `kelvin #9fd8f5` (chłodny niebieski akcent —
  nawiązanie do skali Kelvina/temperatury barwowej światła), `kelvindeep #4a9fd4`, `cream #f5f1ea`, `muted #a39a8d`.
- **Fonty**: `font-display` = Clash Display (Fontshare CDN), `font-sans` = General Sans (Fontshare CDN).
  Nagłówki celowo używają **`font-normal`** (NIE bold) — użytkownik świadomie to cofnął po teście z pogrubieniem,
  uznał że wygląda "zbyt grubo". Nie zmieniać na bold bez wyraźnej prośby.
- **Ikony**: solidne wypełnienie (`fill="currentColor"`, bez obrysu/stroke), kolor amber, BEZ kontenerów/teł wokół
  ikon (usunięte na życzenie) — styl ma nawiązywać do bloków w logo.
- **Animacje**: cząsteczki (`.particle`), aurora w tle hero (`.aurora-blob` × 3 + `.beacon-sweep`), marquee
  kompatybilności, `.led-line` (świecąca linia na górze sekcji), `.shimmer-btn` (połysk na przyciskach CTA).

## ⚠️ Ważne pułapki techniczne (już raz namierzone, nie powtarzać)
1. **`font-700` / `font-800` / `font-600` to NIEPRAWIDŁOWE klasy Tailwind** — nie istnieją, cicho nic nie robią
   (renderują się jako normalne 400). Prawidłowe: `font-bold` (700), `font-extrabold` (800), `font-semibold` (600).
2. **Nigdy nie dodawać `?v=2` czy podobnych query-stringów do `src` obrazków** — jeśli ktoś otworzy plik
   bezpośrednio (`file://`, podwójne kliknięcie), przeglądarka szuka pliku o dosłownej nazwie z `?...` i nie
   znajduje. Jeśli trzeba wymusić odświeżenie cache po podmianie pliku pod tą samą nazwą — powiedz userowi
   żeby zrobił twarde odświeżenie (Ctrl+Shift+R), nie dodawaj query-stringów.
3. **Python nie jest w PATH** w Bash/PowerShell tego środowiska. Pełna ścieżka:
   `C:\Users\48793\AppData\Local\Programs\Python\Python313\python.exe`. Zainstalowane pakiety: `pymupdf`,
   `Pillow`, `numpy`, `scipy` (poppler/pdftoppm NIE jest zainstalowany — do PDF-ów używać PyMuPDF/fitz).
4. **Narzędzie `preview_screenshot` bywa niestabilne** — czasem trzeba ponowić 1-2×. Do weryfikacji stanu DOM
   (kolory, klasy, tekst) lepiej użyć `preview_eval`/`preview_inspect` niż polegać wyłącznie na zrzutach.
5. **Scroll w podglądzie nie zawsze "trzyma się"** między wywołaniami `preview_eval` — przed `window.scrollTo`
   ustawić `document.documentElement.style.scrollBehavior='auto'` (strona ma domyślnie `scroll-behavior: smooth`,
   co gubi się między wywołaniami).
6. **Niestandardowe szerokości viewportu** (np. 1280×800) w `preview_resize` czasem renderują tylko fragment
   ekranu z czarnym paddingiem w zrzucie — to błąd samego narzędzia podglądu, nie strony (zweryfikowane przez
   DOM). Do zrzutów lepiej używać presetów `mobile`/`tablet`/`desktop`.
7. **`preview_click` czasem nie trafia w element** — pewniejszy fallback: `element.click()` przez `preview_eval`.

## Dane produktowe — źródło prawdy
Katalog PDF: `C:\Users\48793\Desktop\Miovo\BMT\Folder\Folder A4 (rev_8).pdf` (67 stron, katalog BMT 2025).
Zawiera pełną ofertę (szynoprzewody, downlighty, żyrandole itd.) — jeśli trzeba dodać kolejne produkty,
to jest źródło realnych specyfikacji i zdjęć (do wyciągania obrazów używać PyMuPDF, `page.get_images()`).

5 produktów już na stronie w sekcji Oferta (zakładki, `#produkty`): M2 Wall Points 6W, M2 Line 24W,
Legacy 4X 15W, Moonlight 80W, M2 Pendant 7W. Dane (JS tablica `products` w `<script>` na dole `index.html`)
i zdjęcia są prawdziwe, wyciągnięte z katalogu.

## Wciąż placeholder / do uzupełnienia (oznaczone `<!-- TODO -->` w kodzie)
- Numer telefonu i godziny otwarcia w stopce.
- Statystyki w pasku pod hero (15+ lat, 500+ projektów, 98%, 30+ miast) — **wymyślone liczby**, trzeba podmienić
  na realne dane klienta.
- Ocena "4.9/5 z 210+ opinii" nad karuzelą opinii — również placeholder.

## Preferencje użytkownika (obserwacje z tej sesji)
- Lubi szybkie działanie bez nadmiernego dopytywania przy zmianach kreatywnych/wizualnych — deleguje zaufanie.
  Dopytuj tylko przy realnych rozwidleniach (np. licencjonowane fonty, sprzeczności w nazwie marki).
- Nie lubi fabrykowanych danych bez oznaczenia — zawsze zostawiaj `TODO` przy wymyślonych liczbach/treściach.
- Bardzo dokładnie ocenia detale wizualne (przezroczystość gradientów, spójność teł zdjęć, kontrast) — oczekuj
  kilku rund poprawek i zawsze weryfikuj wizualnie w podglądzie przed zgłoszeniem "gotowe".
- Woli proste rozwiązania CSS/vanilla JS niż ciężkie frameworki (zainstalowany wcześniej skill
  `website-builder-setup` z Framer Motion/UI-UX-Pro-Max/21st.dev NIE jest używany na tej stronie — świadomy wybór).

## Jak uruchomić podgląd lokalnie
`.claude/launch.json` w folderze nadrzędnym (`C:\Users\48793\Desktop\Claude\`) ma skonfigurowany serwer
`bmt-static-server` (Python `http.server` na porcie 5500, serwujący katalog `bmt-modern-lighting`). Użyj
narzędzia `preview_start` z nazwą `bmt-static-server`.
