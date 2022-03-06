# Részhalmaz-összeg probléma

## A probléma leírása
Van egy ***N*** méretű halmazunk és egy ***T*** célösszegünk, ahol az ***N*** méretű halmaz _részhalmazait_ keressük, amelyek elemeinek összege **megegyezik** a keresett **célösszeggel**.

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
