# DoubleRSA
On se retrouve pour discuter un challenge RSA assez particulier.
## Chiffrement
```py
from Crypto.Util.number import *
import gmpy2

flag = "XXXXXXXX"

p = getPrime(512)
q = getPrime(512)
r = getPrime(512)
n1 = p * q
n2 = p * q * r

e1 = getPrime(20)
e2 = int(gmpy2.next_prime(e1))

m = bytes_to_long(flag)
c1 = pow(m, e1, n1)
c2 = pow(m, e2, n2)

print("n1 = {}".format(n1))
print("n2 = {}".format(n2))
print("e1 = {}".format(e1))
print("e2 = {}".format(e2))
print("c1 = {}".format(c1))
print("c2 = {}".format(c2))
```
## Manuel
```p,q,r```: Three very large primes<br/>
```n1```: First modulus<br/>
```n2```: Second modulus<br/>
```e1```: First key exponent<br/>
```e2```: Second key exponent<br/>
```c1```: First ciphertext<br/>
```c2```: Second ciphertext<br/>
## Soluce
En effet, on a deux modulus dont un qui est composé de 3 nombres premiers.<br/>
On a aussi 2 exponents allignés, pas compliqué à comprendre.<br/>
La première chose qu'on peut se demander: Comment recover ```r``` et sa clé privée à partir des valeurs qu'on a?<br/>
Et bien figurez vous que c'est pas si compliqué que ç'en a l'air.<br/>
### Test
Essayons de reproduire la multiplication des nombres premiers avec des petits nombres.<br/>
```py
>>> n1 = 5 * 4
>>> n2 = 5 * 4 * 9
>>> r = n2 // n1
>>> print(r)
9
```
On peut isoler ```r``` de ```n2``` en divisant ```n2``` par ```n1```.<br/>
```py
>>> n1 = 154935565403645385721284171603184453142670245088181207053410793795106461397612583512380166397225422847494826531450774742551645956962389910358184436862682282246827141646775401052637574142707206015370634903977975983406238952485342823071915176992003895698440907211930889860062847100847368718308879607755780566671
>>> n2 = 1547065595046287231222849370165620395247952974041526328065680194977932152638377271582389978235991816932552754512430114535293810382643386407557754039452917684442506460545446175177440740669282644427762822791495694747715075106206795255985503206131281937023827796976496377661801118129564774626293720203160482334165894311689158006726240034600702778424343751200686210379405029286656771861681691238856769453909401838766947392548275306125960286228437273743154506313483033
>>> r = n2 // n1
>>> print(r)
9985219281420631491950957851274022801091454644758524755532672893128337056039820257283811157323428103614562402318170808820186779244337280644056842166257623
```
```r``` = 9985219281420631491950957851274022801091454644758524755532672893128337056039820257283811157323428103614562402318170808820186779244337280644056842166257623
