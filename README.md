# Mass Effect 1 – čeština a volitelné mody pro Xbox 360

Tento balíček z vlastních čistých herních souborů sestaví:

- kompletní český překlad základní hry,
- české DLC **Strhnout oblohu** a **Stanice Pinnacle**,
- volitelně **Romance stejného pohlaví**,
- volitelně **Spasitele Virmiru**.

Patcher neobsahuje hotové upravené Xbox mapy ani hotové herní DLC kontejnery. Všechny velké herní soubory vznikají až lokálně z uživatelových vstupů. Balíček obsahuje původní zdrojové archivy obou modů a kompatibilní sestavu Legendary Exploreru, aby pro volitelné převody nebylo nutné stahovat další nástroje.

Projekt je určen pro Xbox 360 s RGH/JTAG, Title ID `4D5307E8`, Media ID `572BA75D`, TU0. Neupravuje `default.xex` a nevyžaduje Title Update.

## Zdroje a poděkování

- Původní česká lokalizace Mass Effect 1: **CD Projekt** – [domovská stránka překladu](https://prekladyher.eu/preklady/mass-effect-1.329/).
- [Same-Gender Romances for ME1](https://www.nexusmods.com/masseffect/mods/80), autor **rondeeno**, zdrojová verze 4.1.1.
- [Virmire Savior Mod (LE1)](https://www.nexusmods.com/masseffectlegendaryedition/mods/1212), autor **Vegz**, zdrojová verze 2.03 bez závislosti na LE1 Community Patchi.
- [Legendary Explorer](https://github.com/ME3Tweaks/LegendaryExplorer), projekt **ME3Tweaks**; v balíčku je kompatibilní runtime použitý převodními skripty.

Romance port zahrnuje volby `Same-Gender LI Activates Beacon` a `NPCs Flirt Regardless of Gender`. Oprava scény u majáku odpovídá novější variantě modu: Ashleyina chybná replika „Don't touch her“ se v mužské Kaidanově větvi nepřehraje.

## Co musí dodat uživatel

1. Čistou rozbalenou Xbox 360 verzi Mass Effect 1.
2. PC instalaci původního Mass Effect 1 s aplikovanou češtinou.
3. Neupravený Xbox 360 kontejner DLC Bring Down the Sky.
4. Neupravený Xbox 360 kontejner DLC Pinnacle Station.
5. Při volbě Spasitele Virmiru také čistou instalaci ME1 Legendary Edition.
6. Windows 10/11, 64bitový Python 3 a .NET runtime.
7. Přibližně 10 GB volného místa pro mezisoubory a výsledky.

Přibalené archivy v `source_mods` se rozbalují automaticky. Patcher používá systémový `tar.exe`, který je součástí podporovaných verzí Windows.

## Podoba vstupů

Kořen čisté Xbox hry musí obsahovat alespoň:

```text
Mass Effect 1/
├── default.xex
└── Layer0/
    ├── Maps/
    └── MEInit/GlobalTlk.xxx
```

PC cesta musí vést ke kompletní instalaci obsahující `BioGame/CookedPC/Maps` a české balíčky `.upk`. LE1 cesta může mířit na kořen Legendary Edition, `Game/ME1` nebo přímo `Game/ME1/BioGame`; skript si čistý `CookedPCConsole` najde.

Všechny vstupy musí být čisté. Nepoužívejte Xbox kopii, do které už byla zapsána starší čeština nebo některý z těchto modů.

## Automatické sestavení

Spusťte:

```text
Rebuild_ME1_CZ_From_Clean.bat
```

Průvodce se zeptá na čtyři povinné vstupy a na oba volitelné mody. Cestu k LE1 vyžádá jen při zapnutí Spasitele Virmiru.

Pokročilé spuštění:

```powershell
powershell.exe -NoProfile -ExecutionPolicy Bypass `
  -File ".\tools\Rebuild_ME1_CZ_From_Clean.ps1" `
  -GamePath "D:\Xbox Games\Mass Effect 1 Clean" `
  -PcCzechPath "D:\Games\Mass Effect PC CZ" `
  -BDtSContainer "D:\ME1 DLC Original\BringDownTheSky" `
  -PinnacleContainer "D:\ME1 DLC Original\PinnacleStation" `
  -SameGenderRomances `
  -VirmireSavior `
  -Le1Path "C:\Program Files\EA Games\Mass Effect Legendary Edition" `
  -OutputPath "D:\ME1_X360_Rebuilt"
```

Přepínače `-SameGenderRomances` a `-VirmireSavior` jsou nezávislé. Když jsou zvoleny oba, Spasitel Virmiru se sestaví nad romance mapami, takže se změny v překrývajících se mapách zachovají.

## Co patcher provede

1. Vytvoří český `GlobalTlk.xxx` základní hry.
2. Z české PC instalace sestaví české Xbox dialogy, dialogové volby, meziscény a hlášky.
3. Z obou původních Xbox DLC vytvoří nové české STFS kontejnery a přepočítá jejich hashové tabulky a Content ID.
4. U romance modu porovná přibalený PC mod s čistými PC mapami a změny přenese do čerstvě sestavených českých Xbox map.
5. U Spasitele Virmiru porovná přibalenou verzi 2.03 s čistou LE1 a změny přenese do odpovídajících původních Xbox map.
6. Pro každý volitelný mod postaví malé samostatné podpůrné DLC z uživatelova originálního BDtS kontejneru; mapy v něm nejsou duplikované.

## Výstup

```text
rebuilt_output/
├── 01_Base_Game_CZ/GameFiles/Layer0/...
├── 02_Bring_Down_the_Sky_CZ/<Content ID>
├── 03_Pinnacle_Station_CZ/<Content ID>
├── 04_Same_Gender_Romances_CZ/          # volitelné
│   ├── GameFiles/Layer0/Maps/
│   └── DLC/<Content ID>
├── 05_Virmire_Savior_CZ/                # volitelné
│   ├── GameFiles/Layer0/Maps/
│   └── DLC/<Content ID>
├── optional_mods_validation.json
└── _build/
```

`_build` obsahuje pracovní soubory a po úspěšném testu jej lze smazat. Vstupní adresáře patcher nemění.

## Instalace na Xbox 360

1. Zálohujte celou hru a původní DLC.
2. Obsah `01_Base_Game_CZ/GameFiles` zkopírujte do kořene hry.
3. Pokud jste sestavili romance mod, poté zkopírujte jeho `GameFiles` do kořene hry.
4. Pokud jste sestavili Spasitele Virmiru, zkopírujte jeho `GameFiles` jako poslední.
5. Soubory bez přípony z adresářů `02_...`, `03_...` a volitelných `DLC` nahrajte do:

   ```text
   Hdd1:\Content\0000000000000000\4D5307E8\00000002\
   ```

6. V `00000002` nenechávejte staré nebo duplicitní varianty stejných DLC.
7. Konzoli úplně vypněte a znovu zapněte.

Do `Layer0/MEInit` ručně nekopírujte nic kromě souborů vytvořených základní českou částí. Volitelné mody používají mapové patche a vlastní malé podpůrné DLC.

## Kontrola a řešení problémů

- `Unsupported source` nebo nesouhlas hashů znamená nesprávnou či již upravenou Xbox verzi. Obnovte čisté soubory.
- `PC package was not found` znamená neúplnou PC instalaci nebo chybnou cestu.
- Chyba hledání `CookedPCConsole` znamená, že cesta k LE1 neobsahuje čistou instalaci ME1 Legendary Edition.
- `Fatal Crash Intercepted` nejčastěji způsobí neúplný přenos, souběžně ponechaná stará varianta DLC nebo kopírování pracovního souboru místo finálního Content ID.
- Výsledky a kontroly jsou zapsány ve `validation.json` a `optional_mods_validation.json`. Pokud kontrola selže, soubory na konzoli neinstalujte.

## Právní poznámka

Základní herní soubory, česká PC instalace, LE1 a původní Xbox DLC nejsou součástí balíčku a uživatel je musí dodat z vlastní legálně získané kopie.

Původní mody a Legendary Explorer zůstávají dílem svých autorů.
