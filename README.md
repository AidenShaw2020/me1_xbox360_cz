# Mass Effect 1 – kompletní česká lokalizace pro Xbox 360

Tento návod popisuje vytvoření kompletní české lokalizace z vlastních čistých souborů. Výsledkem je překlad základní hry a obou DLC:

- základní hra včetně menu, kodexu, deníku, dialogů, dialogových voleb, meziscén a hlášek členů družstva,
- **Bring Down the Sky / Strhnout oblohu**,
- **Pinnacle Station / Stanice Pinnacle**.

Konverze je určena pro konzole Xbox 360 s RGH/JTAG. Nepoužívá Title Update a nemění `default.xex`.

## Podporovaná verze

| Položka | Hodnota |
|---|---|
| Title ID | `4D5307E8` |
| Media ID | `572BA75D` |
| XEX version | `5` / TU0 |
| SHA-256 referenčního `default.xex` | `4beb582540010b25032e3a51e4ce84a6fb1a9f381adddd8b2def0dc5d6bf715d` |

Jiná edice nebo již upravené herní soubory nemusí být kompatibilní. Konvertor neznámé vstupy odmítne, místo aby vytvořil potenciálně nefunkční výstup.

## Co budete potřebovat

1. Windows 10 nebo Windows 11 v 64bitové verzi.
2. 64bitový Python 3 dostupný v systémové proměnné `PATH`.
3. Čistou rozbalenou Xbox 360 verzi Mass Effect 1.
4. PC instalaci Mass Effect 1 s aplikovanou českou lokalizací.
5. Originální funkční Xbox 360 kontejner DLC Bring Down the Sky.
6. Originální funkční Xbox 360 kontejner DLC Pinnacle Station.
7. Alespoň přibližně 8 GB volného místa pro pracovní a výsledné soubory.

Potřebné části PC instalace nejsou součástí repozitáře. Skript z nich načte české dialogové tabulky a vytvoří Xbox variantu. Použijte vlastní legálně získanou instalaci hry.

## Příprava vstupů

### Čistá Xbox 360 hra

Připravte lokálně dostupný adresář s rozbalenou hrou. V jeho kořeni musí být `default.xex` a následující struktura:

```text
Mass Effect 1/
├── default.xex
└── Layer0/
    ├── Maps/
    └── MEInit/
        └── GlobalTlk.xxx
```

Nepoužívejte kopii, do které už byla nahrána starší verze češtiny. Pokud jste hru dříve upravovali, nejprve obnovte původní anglické soubory.

### Česká PC instalace

Zadejte kořen celé počeštěné PC hry, například:

```text
D:\Program Files (x86)\Mass Effect
```

Skript prohledává podadresáře rekurzivně. Cesta proto musí vést ke skutečné instalaci obsahující české soubory `.upk`, nikoli pouze k instalačnímu programu češtiny.

### Originální DLC

Připravte dva neupravené STFS soubory, které původní hra na Xboxu správně rozpozná:

- Bring Down the Sky,
- Pinnacle Station.

Použijte přímo soubory DLC, nikoli ZIP archiv. V cílovém adresáři během testování nenechávejte více různých variant stejného DLC.

## Automatická konverze

Nejjednodušší způsob je spustit:

```text
Rebuild_ME1_CZ_From_Clean.bat
```

Postupně zadejte:

1. cestu ke kořeni čisté Xbox 360 hry,
2. cestu ke kořeni počeštěné PC hry,
3. cestu k originálnímu Bring Down the Sky,
4. cestu k originálnímu Pinnacle Station.

Pokročilé spuštění z PowerShellu:

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass `
  -File ".\tools\Rebuild_ME1_CZ_From_Clean.ps1" `
  -GamePath "D:\Xbox Games\Mass Effect 1 Clean" `
  -PcCzechPath "D:\Program Files (x86)\Mass Effect" `
  -BDtSContainer "D:\ME1 DLC Original\BringDownTheSky" `
  -PinnacleContainer "D:\ME1 DLC Original\PinnacleStation" `
  -OutputPath "D:\ME1_CZ_Rebuilt"
```

Konverze zpracovává 297 velkých mapových a dialogových balíčků, takže může trvat delší dobu. Vstupní soubory se nemění; vše vzniká v novém výstupním adresáři.

## Co konvertor provede

1. Z původního Xbox `GlobalTlk.xxx` a českých PC dat vytvoří globální českou tabulku.
2. Znovu sestaví 223 dialogových balíčků základní hry.
3. Znovu sestaví 74 balíčků meziscén a ambientních hlášek.
4. Vytvoří české globální i lokální texty pro Bring Down the Sky.
5. Vytvoří české globální i lokální texty pro Pinnacle Station.
6. Z originálních DLC postaví nové STFS kontejnery a přepočítá jejich L0/L1/L2 hashe, root hash a Content ID.
7. Porovná soubory základní hry s hardwarově ověřenými referenčními hashi.

Pokud některá kontrola selže, výstup nepoužívejte na konzoli.

## Výsledná struktura

Při výchozím nastavení vznikne adresář `rebuilt_output`:

```text
rebuilt_output/
├── 01_Base_Game_CZ/
│   ├── GameFiles/
│   │   └── Layer0/...
│   └── rebuild_report.json
├── 02_Bring_Down_the_Sky_CZ/
│   ├── <nový Content ID>
│   └── validation.json
├── 03_Pinnacle_Station_CZ/
│   ├── <nový Content ID>
│   └── validation.json
└── _build/
```

Adresář `_build` obsahuje mezivýsledky a po úspěšné instalaci jej můžete odstranit.

Při přesně podporovaných vstupech odpovídají finální ověřené soubory těmto hodnotám:

| Součást | Soubor / SHA-256 |
|---|---|
| Základní `GlobalTlk.xxx` | `47363c769e9b3ea261043e2cd9b138e1f52a74510b468ceec6a8a132cdb60604` |
| Strhnout oblohu | `863D1AB3C3C6072258A79ED0EC691652BD40DAB9` |
| Stanice Pinnacle | `554124CF0573A1E51962C116BFB20D96C57983A4` |

Content ID je odvozen z výsledného STFS kontejneru. Pokud se výsledný název nebo validační hash liší, zkontrolujte vstupní DLC a nepokračujte v instalaci.

## Instalace základní hry

1. Zazálohujte celý adresář hry na Xboxu.
2. Obsah adresáře:

   ```text
   rebuilt_output\01_Base_Game_CZ\GameFiles\
   ```

   zkopírujte se zachováním struktury do:

   ```text
   Hdd1:\Games\Mass Effect 1\
   ```

3. Potvrďte přepsání odpovídajících souborů.

Nepřepisujte ani neupravujte:

- `default.xex`,
- `Layer0\MEInit\Coalesced.ini`,
- `Layer0\MEInit\GlobalTlk_PL.xxx`.

## Instalace DLC

Finální soubor z každého z těchto adresářů:

```text
rebuilt_output\02_Bring_Down_the_Sky_CZ\
rebuilt_output\03_Pinnacle_Station_CZ\
```

nahrajte do:

```text
Hdd1:\Content\0000000000000000\4D5307E8\00000002\
```

Od každého DLC ponechte v cílovém adresáři pouze jednu variantu. Starší testovací nebo původní kopii stejného DLC před spuštěním hry přesuňte mimo adresář `00000002`.

Po instalaci konzoli úplně vypněte a znovu zapněte.

## Ověření ve hře

Po spuštění zkontrolujte:

1. české hlavní menu, kodex a deník,
2. české titulky a dialogové volby v nové i uložené hře,
3. titulky meziscén a krátkých hlášek členů družstva,
4. viditelnost obou DLC v nabídce stažitelného obsahu,
5. české názvy a celé popisy obou DLC,
6. načtení misí Strhnout oblohu a Stanice Pinnacle bez pádu hry.

## Řešení problémů

### Python nebyl nalezen

Nainstalujte 64bitový Python 3 a při instalaci aktivujte volbu **Add Python to PATH**. Potom zavřete a znovu otevřete příkazový řádek.

### Chybí LZO runtime

Soubor `work\minilzo-2.10\lzo2.dll` je přiložen. Zkontrolujte, zda jej neodstranil antivirus a zda spouštíte 64bitový Python.

### Unsupported source nebo neodpovídá vstupní hash

Zadaná Xbox hra není čistá podporovaná verze. Obnovte původní soubory se správným Media ID. Nevynucujte pokračování s jinou verzí.

### PC package nebyl nalezen

Cesta nevede ke kompletní počeštěné PC instalaci, případně v ní chybí české `.upk` soubory. Ověřte instalaci češtiny a zadejte kořen celé PC hry.

### Výstup se neshoduje s hardwarově testovanou referencí

Výstup neinstalujte. Zkontrolujte, že používáte čistou Xbox hru, správnou PC češtinu a originální DLC. Soubory neupravujte ručně mezi jednotlivými kroky.

### Hra DLC nevidí

Zkontrolujte Title ID, cílový adresář a to, že je v `00000002` skutečný výsledný soubor bez přípony. Odstraňte duplicitní varianty stejného DLC a restartujte konzoli.

### Fatal Crash Intercepted při načítání DLC

Nejčastější příčinou je neúplný FTP přenos, smíchané varianty DLC nebo použití mezisouboru z `_build`. Nahrajte pouze finální Content ID z adresářů `02_...` a `03_...` a ověřte velikost souboru po přenosu.

## Obnova angličtiny

Konvertor sám vstupní soubory nemění. Pro návrat hry do angličtiny vraťte zálohu původního adresáře hry a odstraňte české Content ID obou DLC. Bez předchozí zálohy základní hry není bezpečná automatická obnova možná.

## Doporučená struktura repozitáře

Celý obsah tohoto adresáře ponechte pohromadě:

```text
06_Rebuild_From_Clean_Originals/
├── README.md
├── Rebuild_ME1_CZ_From_Clean.bat
├── recipes/
├── tools/
└── work/
```

Skripty používají relativní cesty. Nepřesouvejte samostatně pouze `.bat` nebo `.ps1` soubor bez podadresářů `recipes`, `tools` a `work`.

## Právní poznámka

Projekt neposkytuje původní herní soubory. Pro sestavení musíte použít vlastní kopii Xbox 360 hry, vlastní PC instalaci s českou lokalizací a vlastní originální DLC. Výsledné upravené soubory používejte pouze v souladu s licencí hry a právními předpisy ve vaší zemi.
