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

* Patched unsigned int
```
nano somme_unsigned
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


ù
