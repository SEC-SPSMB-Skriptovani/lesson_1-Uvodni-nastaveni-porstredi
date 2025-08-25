# Cvičení 1 - Úvod a nastavení prostředí

V tomto cvičení se zaměříme na úvodní nastavení prostředí potřebného pro práci v rámci kurzu. Cílem je připravit si funkční virtuální stroj s Linux OS Debian 12, nakonfigurovat základní systémové prostředí, seznaámit se s vestavěnými uživatleskými účty a ověřit, že máme k dispozici všechny potřebné nástroje pro následnou práci. 

## Předpoklady  
Na cvičeních budeme používat následující nátroje:  
<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/d/d5/Virtualbox_logo.png" alt="VirtualBox Logo" width="48"/>
  <img src="https://www.debian.org/logos/openlogo-nd-100.png" alt="Debian Logo" width="48"/>
  <img src="https://resources.jetbrains.com/storage/products/company/brand/logos/IntelliJ_IDEA_icon.png" alt="IntelliJ IDEA Logo" width="48"/>
</p>

**VirtualBox** – virtualizační nástroj pro běh operačního systému v izolovaném prostředí  
**Debian 12.ova** – připravený obraz systému Debian 12 určený k importu, případně [ISO soubor](https://www.debian.org/distrib/netinst) instalačního média.  
**IntelliJ IDEA** – vývojové prostředí pro práci se zdrojovým kódem  

## Import VM
1. Otevřete **VirtualBox** a zvolte možnost *Importovat Applianci* `CTRL + I` nebo *Nový stroj* `CTRL + N`.  
2. Pokud máte připravený **.ova** soubor, nahrajte jej přímo.  
   - V opačném případě vytvořte nový VM a připojte ISO s Debianem 12.  
3. Nastavte parametry stroje (doporučeno):  
   - **Paměť RAM**: min. 2 GB (lépe 4 GB)  
   - **Počet CPU**: min. 2 jádra  
   - **Disk**: min. 20 GB dynamicky alokovaného prostoru  
   - **Síť**: NAT
4. Spusťte virtuální počítač (*Virtual Machine* - VM), případně dokončete instalaci Debianu.

## První start systému

### Přihlášení
Přihlas se pomocí dodaných přihlašovacích údajů. V OVA jsou dostupné následující účty:

| Uživatelské jméno | Heslo     | Popis                |
|------------------|-----------|--------------------|
| `root`             | `student`   | *Administrátorský účet s plným přístupem* |
| `student`          | `student`   | *Standardní uživatelský účet*            |

Po prvním spuštění virtuálního stroje je nutné provést několik základních úkonů k ověření funkčnosti prostředí. 
### Ověření připojení k síti  
Nejprve je vhodné ověřit, zda je k dispozici připojení k síti Internet. K tomuto účelu lze využít příkaz:  

```bash
ping google.com
```
### Aktualizace systému
přihlašte se jako administrátor `root` a následně zadej příkaz:
```bash
sudo apt update && sudo apt upgrade -y
```

## Změna layoutu klávesnice na **CZECH QWERTZ** (volitelné)
opět jako `root` zadejte příkaz:
```bash
nano /etc/default/keyboard
```
a změnte rozložení klávesnice na:
``` bash
XKBLAYOUT="cz"
XKBMODEL="pc105"
```
následně aplikaci ukončíme `CTRL + X`:

<img width="500" height="116" alt="image" src="https://github.com/user-attachments/assets/59f5fd75-923f-429a-b2a0-ddc6ae3ae1e0" />

a provedené změny potvrdíme pomocí `A` (*Ano*), nebo `Y` (*Yes*), které bude funkovat i pro anglickou verzi.

## Instalace Virutalbox přídavků pro hosta
**VirtualBox Guest Additions** (přídavky pro hosta) jsou speciální sada ovladačů a nástrojů, které zlepšují integraci mezi hostitelským systémem a virtuálním strojem. U Debianu 12 (a jiných Linux distribucích) poskytují několik užitečných funkcí.

### Hlavní funkce VirtualBox Guest Additions

VirtualBox Guest Additions (přídavky pro hosta) jsou speciální sada ovladačů a nástrojů, která zlepšuje integraci mezi hostitelským systémem a virtuálním strojem. U Debianu 12 poskytují několik užitečných funkcí:

- **Sdílené složky**  
  Pro přidání sdílené složky mezi hostitelem a Debianem je nutné mít nainstalované Guest Additions. Bez nich VirtualBox „nevidí“ sdílené složky jako připojitelný disk.

- **Lepší grafika a automatické přizpůsobení rozlišení**  
  Host může dynamicky měnit velikost okna a Debian se mu automaticky přizpůsobí.

- **Sdílená schránka**  
  Umožňuje kopírování a vkládání textu mezi hostitelským systémem a virtuálním strojem.

- **Podpora myši „bez zachycení“**  
  Myš se hladce pohybuje mezi hostem a hostitelem, není třeba ji „odchytnout“ kombinací kláves.

- **Rychlejší grafický výkon**  
  Zajišťuje 3D akceleraci a lepší rendering grafiky.

### Připojení ISO Guest Additions

1. V menu VirtualBoxu zvolte: **Zařízení → Přidat obraz disku Guest Additions…**  
2. VirtualBox připojí ISO jako CD-ROM do virtuálního systému.  

<img width="300" height="696" alt="image" src="https://github.com/user-attachments/assets/27b61688-17f7-4dcd-b027-c2239ff5da94" />

### Instalace Guest Additions
V systému Debian 12 proveďte následující příkazy:
```bash
mount /dev/cdrom /media/cdrom
cd /media/cdrom
./VBoxLinuxAdditions.run
```
Následně systém restartujeme pomocí příkazu `reboot`.

## Přidání  Sdílené složky z hostitelského počítače
1. Jako první krok vypneme Virtuální počítač. Lze pouze zavřít okno aplikace s možností **Vypnout počítač**, případně příkazem `poweroff`.
2. Vytvoříme na osobním počítači složku do které budeme umisťovat naše budoucí scripty a kterou budeme mapovat do virtuálního počítače. *Např v osobním adresíři studenta na sdídelném disku*
3. V **nastavení** Virtualboxu sekci **Sdílené složky** vytvořte novou sdílenou složku odkazující na námi vytovřenou složku s **Mount point** (adresář, kde bude tato složka dostupná uvnitř virtuálního stroje) na `/home/student/scripts`.
   Nezapomeňte zaškrtnout **Automatické připojení**.
<img width="300" height="578" alt="image" src="https://github.com/user-attachments/assets/59cb1d25-1af0-4cd5-b9a9-716f333f1dc7" />

4. Opět spustíme virtuální počítač.

### Testování přístupu ke sdílené složce  

1. V systému **Windows** vytvořte testovací soubor (*například nový TXT dokument*) v namapovaném adresáři.  
2. Přihlaste se jako **root** a ověřte přístup do sdílené složky:  

```bash
su -
cd /home/student/scripts
ls -All
```

Výstup by měl obsahovat nově vytvořený soubor.  

3. Přihlaste se jako uživatel **student** a zopakujte postup:  
   - ***Přístup bude pravděpodobně zamítnut.***  
   - Pokuste se proto provést stejný příkaz pro nadřazenou složku:  

```bash
cd /home/student
ls -All
```

Výstup může vypadat následovně:  

```bash
drwxrwx--- 1 root vboxsf , 25. srp 13:04 scripts
```

| Pole             | Hodnota       | Význam                                                                  |
|------------------|---------------|-------------------------------------------------------------------------|
| Typ a práva      | drwxrwx---    | `d` = adresář, práva: vlastník `rwx`, skupina `rwx`, ostatní `---`      |
| Počet odkazů     | 1             | Počet odkazů (hard linků) na adresář                                    |
| Vlastník         | root          | Uživatel, který adresář vlastní                                         |
| Skupina          | vboxsf        | Skupina, které adresář náleží (např. VirtualBox Shared Folders)         |
| Velikost / inode | –             | U adresářů není velikost souboru obvykle relevantní                      |
| Datum a čas      | 25. srp 13:04 | Datum a čas poslední změny obsahu adresáře                              |
| Název            | scripts       | Název adresáře nebo souboru                                             |

Vlastníkem adresáře je uživatel **root** a skupina **vboxsf**.  
V systému Linux není možné přiřadit více vlastníků, proto je nutné přidat uživatele `student` do skupiny `vboxsf`.  

Příkaz pro přidání:  

```bash
sudo usermod -aG vboxsf student
```

> **Poznámka:** Uživatel `student` se musí po přidání do skupiny odhlásit a znovu přihlásit, aby se změna projevila.

4. Pokud je přístup do složky `/home/student/scripts` funkční a váš soubor je viditelný, vytovořte nový soubor `test.sh` příkazem:
```bash
touch test.sh
```
a porovnejte seznam souborů v hostitelském počítači (Windows) ve vaší složce. 


## Seznam použitých příkazů

| Příkaz                                | Popis                                                                 |
|--------------------------------------|----------------------------------------------------------------------|
| `ping google.com`                    | Ověření připojení k síti Internet pomocí ICMP požadavků.              |
| `sudo apt update && sudo apt upgrade -y` | Aktualizace seznamu balíčků a instalace nejnovějších verzí.        |
| `nano /etc/default/keyboard`         | Otevření konfiguračního souboru klávesnice v editoru `nano`.          |
| `mount /dev/cdrom /media/cdrom`      | Připojení CD-ROM (s ISO Guest Additions) do adresáře `/media/cdrom`. |
| `cd /media/cdrom`                    | Přechod do adresáře s připojeným CD-ROM.                             |
| `./VBoxLinuxAdditions.run`           | Spuštění instalačního skriptu pro VirtualBox Guest Additions.        |
| `reboot`                             | Restartování systému.                                                 |
| `poweroff`                           | Vypnutí virtuálního počítače.                                         |
| `su -`                               | Přihlášení jako uživatel `root` (administrátor).                      |
| `cd /home/student/scripts`           | Přechod do adresáře `scripts` v uživatelském adresáři studenta.       |
| `ls -All`                            | Výpis obsahu adresáře se zobrazením detailních informací.             |
| `cd /home/student`                   | Přechod do domovského adresáře uživatele `student`.                   |
| `sudo usermod -aG vboxsf student`    | Přidání uživatele `student` do skupiny `vboxsf` (pro sdílené složky). |
| `touch test.sh`                      | Vytvoření prázdného souboru `test.sh`. 

