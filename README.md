# Linux_ukoly
Úkol 1
1. Kterých 5 států mělo nejvíce účastníků?
Cat athletes (přečte athletes, abych věděl, jakou má strukturu) | cut –d „,“ –f 4 (zobrazí jen čtvrtý sloupec, ve kterém jsou země) | sort –i (seřadí za sebou bez ohledu na velikost písmen)| uniq –c (vypíše počet duplicit na začátek každého řádku) | sort –n (seřadí podle počtu duplicit vzestupně) | tail -5 (ukáže 5 nejčetnějších hodnot)
2. Kolik žen a kolik mužů se účastnilo?
Cat athletes (přečte athletes, abych věděl, jakou má strukturu) | cut –d „,“ –f 2 (zobrazí jen druhý sloupec, ve kterém jsou pohlaví) | sort –i (seřadí za sebou bez ohledu na velikost písmen)| uniq –c (vypíše počet duplicit na začátek každého řádku)
3. Který stát měl nejvíce plavců (M) a který měl nejvíce plavkyň (F)? (případně více států v případě shody)
Cat athletes | cut –d „,“ –f 2-4 | grep –i swimming | sort –i | cut –d „,“ –f 1,3 | uniq –c | sort –n | tail -5
4. Kolik druhů sportů, které obsahují slovo "ball" je v souboru?
Cat athletes | cut –d „,“ –f 3 | grep ball | sort –i | uniq –d | wc –l (spočte počet slov)

5. Který sportovec má na počet znaků nejdelší jméno a z jakého je státu?
Cat athletes | cut –d „,“ –f 1 | wc –L (najde nejdelší řádek ve sloupci se jménem a vypíše počet znaků)
Cat athletes | cut –d „,“ –f 1 | grep ‘.\{35\}’ (najde řádek s počtem znaků 35 a vice a vypíše jej) našlo dvě jména
Cat athletes | cut –d „,“ –f 1,4 | grep –i reynoso
Cat athletes | cut –d „,“ –f 1,4 | grep –i santos 
Cat athletes | cut –d „,“ –f 1,4 | grep –E –i ´simoes|reynoso´ (tohle rovnou najde oba dva bez ohledu na velikost písmen a slouží jako NEBO

Úkol 2
1.	/usr/share/man/man (tam jsou manuály)
2.	Mkdir mans_with_et_in_name (vytvoření složky)
3.	Find /usr/share/man/man/man7 /usr/share/man/man8 | grep „et“ | sort –t / -k6 > mans_with_et_in_name.txt (aby to fungovalo, musím být v mans … složce, jinak to vypíše absolutní cestu a pak to musím mazat, dá se to smazat rychle ve vi Ctrl+V)
4.	Find /usr/share/man/man/man7 /usr/share/man/man8 | grep „et“ | sort –t / -k6 | xargs – i cp {} „path“ (xargs udělá následující příkaz ze stdin-musím užít protože cp to neumí převzít přes pipu) – na tohle se ještě podívat, přestože už je to asi víc pokročilý. 
5.	Grep –w (přesná shoda hledaného slova)Rl (vypíše pouze názvy filů) group v mans_containing_word_group > mans_containing_word_group.txt
(pořádně si přečíst úkol, aby se udělalo vše, jak má. Vyrobit složky, tak jak je napsáno, hlavně složku z „group“). 
6.	Tar –z(dělá gzip) cvf (vytvoří novou složku a napíše, co udělal)   mans_containing_word_group.gz (jméno taru)   mans_containing_word_group (zdrojový adresář)
7.	Tar –cvJ (xz) f mans.tar.xz (a dále názvy souborů, který chci přidat)

Úkol 3
1.	Create the „sidious“ account
Sudo useradd sidious

2.	Add Lord Sidious t the group „sith“
Sudo usermod –G sith sidious

3.	Switch to Lord Sidious (from this moment on you must not use the „root“ account!)
Sudo su sidious

4.	Check that darth plagueis aka „plaggy account exists
Cat /etc/passwd | tail -5 

5.	There is a supsicious file somewhere in the systém owned by the group „sith“ which will help you to destroy your master
Find / -group „sith“ 
V logu byl soubor .gz
Gunzip –c /usr/local/share/man/man9x/secret.9.gz
Výsledkem byl password: power

6.	With the information in the file:
6a. Get passwordless sudo
Pod skupinu wheel doplněno
#sith ALL=(ALL) NOPASSWD: ALL
6b. Remove Darth Plagueis from the system along with his home direktory
Sudo –r plaggy (kontrola cat /etc/passwd | tail -5) – plaggy zmiznul 

7.	Find all files owned by the former user „plaggy“
Find / -nouser 

8.	Take the files and re-arrange their contents in order to formulate the Sith philosophy and save it under „/var/tmp/sith_rule.txt“

Sudo cat + cesta na všechny secret1-8
Přes vi předěláno na two there should be no more no less

9.	You’re the almighty Darth Sidious, and no feeble user shall challenge your authoryty – prevent all regular users from being able to swithc to a different (any) user. 

Sudo chmod o-x /bin/su (zamezí spouštění binárky su). Skoro všechno je binárka a tímhle to jde zakázat. 

10.	All privileged operations from this moment on will have to be performed from the sidiou account – thats becaouse you control the galasy. When you need to switch to a different account, use sudo.

11.	Create users Han Solo (codeame han UID:5010) and Chewbacca (codename chewie)
Useradd han, useradd chewie, usermod –u 5010 han

12.	Han and Chewie are part of the „smugglers“ group
Sudo usermod –G smugglers han, sudo usermod –G chewie

13.	Smugglers have a direktory somewhere in the systém where tehy can share intel, find it!
Find / -group smugglers (našlo složku s absolutní cestou /smugglers)
14.	Make Han owner of the direktory
Sudo chown –cR (vypíše co udělal a je to rekurzivně) han /smugglers

15.	Switch to Han solo’s user
Sudo su han

16.	Han is inclusive and wants all intel to be owned by the „smugglers“ group, make sure the roup ownership is forced to all newly created files in that directory
Chmod g+s /smugglers, vyzkoušeno přes touch a funguje (nastavení speciálního bitu)

17.	Han prepared last details of their stint and saved the following time stamp to „ledger/job.txt“ under the discovered smugglers“ home directory:
``18:18 Christmas Eve 2020 in seconds since the Big Bang aka Epoch``
Han doesn’t trust people easily and so he also created a backup of the file in his home directory

Touch –d „2020-12-24 18:18:00“ job.txt
Touch –t 202012241818.00 job.txt 
Cp jobs.txt ~ 
Jen jsem uložil do toho souboru timestamp.

18.	Switch back to sidious
Ctrl + D

19.	Create a Lando (codename lando)
Sudo useradd lando

20.	Lando is not a smuggler!


21.	Switch to Lando’s user
Sudo su lando


