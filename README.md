# CPP smrdí, tady je důkaz

Nebude to moc formální a budu kašlat na dobrej styl vypravování a na pravopis.

Budu srovnávat C s C++ primárně v oblastech, kde C++ hází víc klacků pod nohy než C, ale nedá se říct, že by to bylo fér srovnání.
Pokud se jedná o velkej neembeded projekt, tak je samozřejmě C++ menší zlo než C, ale ve srovnání s Rustem C++ stejně dostane na prdel.

Začal bych zlehounka, a to u overloadění bitshiftu (`<<` a `>>`)
Streamy jsou sice fajn věc, ale když se s nima pracuje přes overloaděný bitshifty, tak z toho vzniká hodně pastí.

Následující konstrukce je jednou z nich, člověk musí myslet na závorky, jinak se mu z toho stanou reálný bitshifty a negace a to pak rapidně mění význam.
```cpp
if (!std::cin >> ...)
```

Člověk by doufal, že vytiskne výsledek logickýho andu, ale ani omylem. Na cpp reference je na poměry cpp reference relativně hezky popsanýá operator precedence.
```cpp
std::cout << A && B ...
```
Jelikož `<<` má větší prioritu než `&&`, tak se provede `std::cout << A` a ve 2. kroku se provede `&&` a jelikož ten potřebuje nějaký číslo, tak se ten stream převede na `true`, pokud neselhal a vyhodnotí se (selhání/neselhání `&&` B) a výsledek je `1` nebo `0`.







Teď na takový odlehčení si dáme alokaci a inicializaci, to je takový lehký téma na odlehčení, kde není co pokazit. ALE C++ SI ŘEKLO, TO TEDA NE A VÝSLEDEK JSOU SLZY V MÝCH OČÍCH.

v C je pokud vim pouze int x = 5; a nebo neinicializace int x;
    dá se v tom lehce udělat chyba, páč nováček by počítal, že int x; bude x 0, což možná i jo, ale zároveň je o dost větší pravděpodobnost, že tam nebude, protože to bude číst bity z paměti po předchozim programu na nějaký random adrese a tam pravděpodobně nebude 0;

a c++ tohle mega komplikuje až to bolí.

    int x{5}; pokud se do závorek nic nedá, tak se hodnota inicializuje na 0, což zabrání dosti chybám.
    int x(5); tohle volá konstruktor co intu hodí 5ku, ale fakt nechápu.
    int *p = new int(5); alokuje to int a hodí ho to na 5ku
    int *pp = new int[10]; alokuje to pole 10ti intů
    ta alokace by byla asi v pohodě, kdyby k ní byl jen 1 free, teda vlastně delete
    existuje delede a delete[] a kompilátor neumí pracovat jen s jednim a měnit ho podle toho jak byly data naalokovaný, ale musí na to myslet uživatel...
    delete p;
    delete[] pp;


overloadění funkcí v c++ je hell, pokud tam člověk všude necpe explicita, ale to dělá málo kdo

někdo by ale mohl namítnout: mně to ale vyhovuje, že mám 1 funkcu který má 2 deklarace na float a int a já jí volám pod stejnym ménem, ale pozor, i v této oblasti má Cčko navrch, má od c11 makro _Generic a to je sice lehce víc hard na napsání, ale pak funguje jak má a kompiler s -Wall -Wextra -pedantic si vyprošuje přetypování.




a když sme u toho přetypovávání, tak v c bylo pouze (typ) a to bylo vše, ale c++ zachovává jak Cškovký, tak přidává mega dalších (i když možná lepších, ale kdo se v tom pak má asi vyznat co?)
c++ má: static_cast<typ> bezpečnej, čekuje se při kompilaci, jeslti to není blbost
        dynamic_cast<typ> tohle sem ještě nevyužil, ale dovedu si představit, že se to může hodit, takže zatim nebudu hejtit
        const_cast<typ> tohle bere const proměnejm const a k tomu momentálně nevidim jediný dobrý využití
        reinterpret_cast<typ> tohle taky moc nechápu proč to tam je (chová se to asi jako klasickej Cčkovej()) s tim, že ten Cčkovej je teda furt o něco horší, protože se chová jak kombinace všechn dohromady, ale aspoň se člověk pak nemusí učit 5 castů a stačí jen 1


Vubec, ale doopravdy vubec mně neleze do hlavy friend funkce, tak když je ta metoda tak blbě navržená, že potřebuje friend funkci, tak je na čase se zamyslet.
Snad všechno co dělá friend fukce se dá udělat pomocí public metody a normální funkce (kromě overloadění operátorů, tam lepší možnost než friend funkce není pokud vim) a getery a setery taky nepovažuju za ideální, ale friend fakt 100% porušuje zapouzdření a nedává mně to smysl proč by to tam kdy někdo přidával.

A když sme u těch metod, tak proč sou na to v c++ 2 skoro totožný keywordy a to class a struct kde jedinej rozdíl je ve viditelnosti, že struct je by default public a class je private. Chápu, že struct je relikvie z kompatibility kvůli C, a používat by se měla spíš clasa co je defaultně hezky private, ale stejně to není nijak konzistentní a člověk musí umět obojí i když by stačilo jen jedno z toho.
