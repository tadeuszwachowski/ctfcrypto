---
layout: writeup
title: "Franklin Reiter"
---

## Franklin Reiter

If $e$ and $\varphi(n)$ not coprime - $gcd(e,\varphi(n)) \neq 1$ - then, we must find $d$ in other way.

```python
# use SageMath

from Crypto.Util.number import long_to_bytes, bytes_to_long

# r is the known difference
# c1 - smaller ciphertext
# c2 - larger ciphertext (c1+r in some way)
def franklinReiter(n,e,r,c1,c2):
    R.<X> = Zmod(n)[]
    f1 = X^e - c1
    f2 = (X + r)^e - c2
    # coefficient 0 = -m, which is what we wanted!
    return Integer(n-(compositeModulusGCD(f1,f2)).coefficients()[0])

# GCD for rings  
def compositeModulusGCD(a, b):
    if(b == 0):
        return a.monic()
    else:
        return compositeModulusGCD(b, a % b)

# We know c1, c2, r, e, n
# c1 = pow(m + 0, e, n)
# c2 = pow(m + r, e, n)

def frbrute(n,e,r,c1,c2,difference=b"aaaaaaaaaa",known_pt=b"flag{"):
    for i in range(150):
        r = bytes_to_long(difference)*(256**i)
        flag = franklinReiter(n,e,r,c1,c2)
        flag_text = long_to_bytes(flag)
        if known_pt in flag_text:
            print(flag_text)
            break

# change accordingly
n = 51482698748683767088087774270468156789100783641624785984084944863403828221503510365647069203224514944133755999305691830273348754501244540637720175453826222195986788740756721867756000903482921840078455512554853981304825785510942041958289855014883755566433382271880955982668769167111413833106363196394414775723
e = 7
c1 = 39543566838040231239939164803218710613860645995020757373962281275377085927966049379744197052397001009854135889517120893834339242951683165219271078557757906181200134496843382014432758096445044636698554061314566046396547732647265656862210402239966335508430547937762410030734921297166737744160611698564562209262
c2 = 41946100253688314489554460200533212544861235453293689162120385015587544249894510737558568165826824266673923747032483973391038813083195661342878853206935911092078295829114403226841068488887869839383208080644706526631222059077684047373505456924287431408824857660746171269417541645046015662527642772050914041123
difference = b"aaaaaaaaaa"
known_pt = b"srdnlen{"


r = bytes_to_long(difference)
frbrute(n,e,r,c1,c2,difference,known_pt)


```

