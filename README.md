# Részhalmaz-összeg probléma



## A probléma leírása
Van egy ***N*** méretű halmazunk és egy ***T*** célösszegünk, ahol az ***N*** méretű halmaz _részhalmazait_ keressük, amelyek elemeinek összege **megegyezik** a keresett **célösszeggel**.



## Példa probléma
### Bemenet
```
Halmaz = {1, 2, 3, 4, 5, -6, 7}
Keresett összeg = 7
```
### Megoldás
```
Létezik megoldás!
[1 2 4]
[3 4]
[2 5]
[1 3 4 5 -6]
[7]
[1 2 3 -6 7]
[2 4 -6 7]
[1 5 -6 7]
```



## A forráskód értelmezése
A forráskód két könyvtárat tartalmaz: **iostream** és **fstream**. Az **iostream** segít kezelni a ki- és bementei információkat; az **fstream** segít _szöveges állományba_ írni a megoldásunkat.

Maga a kód egy "Reszhalmaz" nevű **osztályon** belül van meghatározva a **könnyű** javíthatóság és bővíthetőség érdekében.

Az osztály három funkciót tartalmaz: **kiir()**, **reszhalmaz_kereses()** és **megoldas()**.
### kiir()
```c++
void kiir(int eredmeny[], int helyzet, int meret)
{
        ki << "[";
        for (int i = 0; i < meret; ++i)
        {
            if (eredmeny[i] != INT_MAX)
                ki << " " << eredmeny[i] << " ";
        }
        ki << "]\n";
}
```
Mielőtt kiírjuk az eredményhalmazunkat az állományba, ellenőrizzük, hogy az eredményhalmaz eleme **nem egyenlő** a maximális egész értékkel. A kiírt elemeket **két szögletes zárójel** közé írjuk, majd egy másik megoldásnál **ugyanezt végrehajtjuk**, csak egy új sorban.

### reszhalmaz_kereses()
```c++

	void reszhalmaz_kereses(int halmaz[], int eredmeny[], int osszeg, int meret, int seged, int helyzet)
	{
		if (helyzet == -1)
			return;

		this->reszhalmaz_kereses(halmaz, eredmeny, osszeg, meret, seged, helyzet - 1);
		eredmeny[helyzet] = halmaz[helyzet];

		if (seged + halmaz[helyzet] == osszeg)
			this->kiir(eredmeny, helyzet, meret);

		this->reszhalmaz_kereses(halmaz, eredmeny, osszeg, meret, seged + halmaz[helyzet], helyzet - 1);
		eredmeny[helyzet] = INT_MAX;
	}
```
Az alprogramunk több paraméter formájában kapja meg az információt, mivel ez egy **rekurzívan** megoldott probléma. Elsősorban megnézzük, hogy érvényes poziciót kaptunk-e. Utána egy **rekurzív hívást** hajtunk végre egy egyel visszábbi pozíción, megnézzük, hogy helyes-e, majd még egy rekurzív hívást hajtunk végre. Legvégül az eredményhalmazt **egyenlővé tesszük** a maximális egész értékkel.

### megoldas()
```c++
	void megoldas(int halmaz[], int meret, int osszeg)
	{
		if (meret <= 0)
			return;

		int eredmeny[meret];

		for (int i = 0; i < meret; ++i)
			eredmeny[i] = INT_MAX;

		ki << "A kívánt összegre való összes lehetséges megoldás:\n";
		this->reszhalmaz_kereses(halmaz, eredmeny, osszeg, meret, 0, meret - 1);
	}
```
Ez az alprogram maga az **illesztőprogram**, ami összefoglal mindent és végrahajtja az adott feladatot. Először megnézzük, hogy **érvényes-e** a megadott halmaz. Utána az azelőtt deklarált halmazt **feltöltjük** maximális egész értékekkel és meghívjuk a "**reszhalmaz_kereses**" alprogramot.



## Forráskód
A feladat forráskódja C++ nyelvben íródott.
```c++
#include <iostream>
#include <fstream>

using namespace std;

ofstream ki("megoldas.txt");

class Reszhalmaz{
public:
    void kiir(int eredmeny[], int helyzet, int meret)
    {
        ki << "[";
        for (int i = 0; i < meret; ++i)
        {
            if (eredmeny[i] != INT_MAX)
                ki << " " << eredmeny[i] << " ";
        }
        ki << "]\n";
    }

	void reszhalmaz_kereses(int halmaz[], int eredmeny[], int osszeg, int meret, int seged, int helyzet)
	{
		if (helyzet == -1)
			return;

		this->reszhalmaz_kereses(halmaz, eredmeny, osszeg, meret, seged, helyzet - 1);
		eredmeny[helyzet] = halmaz[helyzet];

		if (seged + halmaz[helyzet] == osszeg)
			this->kiir(eredmeny, helyzet, meret);

		this->reszhalmaz_kereses(halmaz, eredmeny, osszeg, meret, seged + halmaz[helyzet], helyzet - 1);
		eredmeny[helyzet] = INT_MAX;
	}

	void megoldas(int halmaz[], int meret, int osszeg)
	{
		if (meret <= 0)
			return;

		int eredmeny[meret];

		for (int i = 0; i < meret; ++i)
			eredmeny[i] = INT_MAX;

		ki << "A kívánt összegre való összes lehetséges megoldás:\n";
		this->reszhalmaz_kereses(halmaz, eredmeny, osszeg, meret, 0, meret - 1);
	}
};

int main()
{
	Reszhalmaz feladat;
	int meret, osszeg;

	cout << "Add meg a kivant halmaz meretet! ";cin >> meret;
	cout << "\nAdd meg a halmaz elemet! \n";

	int halmaz[meret];

	for(int i = 0; i < meret; i++)
        cin >> halmaz[i];

    cout << "\nAdd meg a keresett osszeget! "; cin >> osszeg;

	feladat.megoldas(halmaz, meret, osszeg);

	cout << "\nA feladat megoldasa a program mappajaban keresendo, 'megoldas.txt' nev alatt! \n";

	return 0;
}

```



## Névjegy
György Norbert | "Salamon Ernő" Ginázium | XI.A | 2022.03.06
