# overflow
* Unsigned integer overflow
```
nano somme_unsigned.c
```
```
#include <stdio.h>

// Fonction qui calcule la somme de deux entiers non signés
unsigned int somme_unsigned(unsigned int a, unsigned int b) {
    return a + b;
}

int main() {
    unsigned int num1, num2, result;

    // Demande des valeurs à l'utilisateur
    printf("Entrez le premier entier non signé : ");
    scanf("%u", &num1);

    printf("Entrez le deuxième entier non signé : ");
    scanf("%u", &num2);

    // Calcul de la somme
    result = somme_unsigned(num1, num2);

    // Affichage du résultat
    printf("La somme de %u et %u est : %u\n", num1, num2, result);

    return 0;
}

```
```
gcc -o somme_unsigned somme_unsigned.c
```
```
chmod +x somme_unsigned
```
```
./somme_unsigned
```
Entrez le premier entier non signé : 25 </br>
Entrez le deuxième entier non signé : 75 </br>
La somme de 25 et 75 est : 100 </br>
```
rm -rf somme*
```

* Patched unsigned int
```
nano somme_unsigned.c
```

```
#include <stdio.h>
#include <stdbool.h>

// Fonction pour calculer la somme avec détection d'overflow
bool somme_unsigned(unsigned int a, unsigned int b, unsigned int *result) {
    *result = a + b;
    // Détection d'overflow : si le résultat est inférieur à l'un des opérandes
    if (*result < a || *result < b) {
        return false; // Overflow détecté
    }
    return true; // Pas d'overflow
}

int main() {
    unsigned int num1, num2, result;

    // Demande des valeurs à l'utilisateur
    printf("Entrez le premier entier non signé : ");
    scanf("%u", &num1);

    printf("Entrez le deuxième entier non signé : ");
    scanf("%u", &num2);

    // Calcul de la somme avec vérification d'overflow
    if (somme_unsigned(num1, num2, &result)) {
        printf("La somme de %u et %u est : %u\n", num1, num2, result);
    } else {
        printf("Erreur : Overflow détecté lors de la somme de %u et %u !\n", num1, num2);
    }

    return 0;
}
```
```
gcc -o somme_unsigned somme_unsigned.c
```
```
chmod +x somme_unsigned
```
```
./somme_unsigned
```
Entrez le premier entier non signé : 4294967295 </br>
Entrez le deuxième entier non signé : 1 </br>
Erreur : Overflow détecté lors de la somme de 4294967295 et 1 ! </br>

```
rm -rf somme*
```

* Signed somme_signed
```
nano somme_signed.c
```
```
#include <stdio.h>

// Fonction qui calcule la somme de deux entiers signés
int somme_signed(int a, int b) {
    return a + b;
}

int main() {
    int num1, num2, result;

    // Demande des valeurs à l'utilisateur
    printf("Entrez le premier entier : ");
    scanf("%d", &num1);

    printf("Entrez le deuxième entier : ");
    scanf("%d", &num2);

    // Calcul de la somme
    result = somme_signed(num1, num2);

    // Affichage du résultat
    printf("La somme de %d et %d est : %d\n", num1, num2, result);

    return 0;
}
```
```
gcc -o somme_signed somme_signed.c
```
```
chmod +x somme_signed
```
```
./somme_signed
```
```
rm -rf somme*
```



* patched somme_signed

Overflow positif : </br>
Si a > 0, b > 0 et le résultat (a + b) est négatif, cela indique un dépassement de la capacité maximale des entiers (INT_MAX).  </br> </br>

Underflow négatif :  </br>
Si a < 0, b < 0 et le résultat (a + b) est positif, cela indique un dépassement de la capacité minimale des entiers (INT_MIN).  </br> </br>

```
nano somme_signed.c
```
```
#include <stdio.h>
#include <stdbool.h>
#include <limits.h>

// Fonction pour calculer la somme d'entiers signés avec détection d'overflow
bool somme_signed(int a, int b, int *result) {
    *result = a + b;

    // Détection d'overflow
    if ((a > 0 && b > 0 && *result < 0)) {
        return false; // Overflow positif
    }

    // Détection d'underflow
    if ((a < 0 && b < 0 && *result > 0)) {
        return false; // Underflow négatif
    }

    return true; // Pas d'overflow ni d'underflow
}

int main() {
    int num1, num2, result;

    // Demande des valeurs à l'utilisateur
    printf("Entrez le premier entier signé : ");
    scanf("%d", &num1);

    printf("Entrez le deuxième entier signé : ");
    scanf("%d", &num2);

    // Calcul de la somme avec vérification d'overflow/underflow
    if (somme_signed(num1, num2, &result)) {
        printf("La somme de %d et %d est : %d\n", num1, num2, result);
    } else {
        printf("Erreur : Overflow ou underflow détecté lors de la somme de %d et %d !\n", num1, num2);
    }

    return 0;
}
```
```
gcc -o somme_signed somme_signed.c
```
```
chmod +x somme_signed
```

* overflow float
```
nano float_overflow.c
```
```
#include <stdio.h>
#include <math.h>

int main() {
    float x;

    // Demander la valeur de x à l'utilisateur
    printf("Entrez une valeur pour x : ");
    scanf("%f", &x);

    // Vérifier si x est égal à 0.000007
    if (x == 0.000007) {  // Tolérance d'égalité pour float
        printf("OK\n");
    } else {
        printf("NON OK\n");
    }

    return 0;
}
```
```
gcc -o float_overflow float_overflow.c
```
```
chmod +x float_overflow
```
```
./float_overflow
```
```
rm -rf float_overflow
```

* overflow float_patched
```
#include <stdio.h>
#include <math.h>

int main() {
    float x;

    // Demander la valeur de x à l'utilisateur
    printf("Entrez une valeur pour x : ");
    scanf("%f", &x);

    // Vérifier si x est égal à 0.000007
    if (fabsf(x - 0.000007f) < 0.000001f) {  // Tolérance d'égalité pour float
        printf("OK\n");
    } else {
        printf("NON OK\n");
    }

    return 0;
}
```
```
gcc -o float_overflow float_overflow.c
```
```
chmod +x float_overflow
```
```
./float_overflow
```
Explications des modifications : </br>
Pourquoi ne pas utiliser x == 0.000007f directement ? </br></br>

En raison de la précision limitée des nombres flottants (float), les calculs peuvent entraîner des imprécisions. </br>
Par exemple, un nombre entré par l'utilisateur ou résultant d'un calcul peut ne pas être exactement égal à 0.000007 en binaire. </br>
Utilisation de fabsf() : </br></br>

La fonction fabsf() (de <math.h>) calcule la valeur absolue d'un flottant.</br>
La condition fabsf(x - 0.000007f) < 0.000001f vérifie si la différence entre x et 0.000007f est suffisamment petite (moins de 0.000001 dans cet exemple) pour être considérée comme égale. </br>
Tolérance (epsilon) : </br>

La valeur 0.000001f est utilisée comme marge d'erreur pour les comparaisons flottantes. Elle peut être ajustée en fonction de la précision requise. </br>

Entrez une valeur pour x : 0.000007 </br>
OK

Entrez une valeur pour x : 0.0000075 </br>
NON OK




