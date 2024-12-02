# SimpleRSA

We are given an RSA algorithm implementation in `chall.py` and the values of `p`, `q` and `c` in `out.txt` as follows:<br/>

### chall.py
```Python3
from secret import flag
from Crypto.Util.number import bytes_to_long, getPrime

flag = bytes_to_long(flag)
p = getPrime(2048)
q = getPrime(2048)
c = pow(flag, p, q)  # i believe this is the fancy rsa encryption?
print(f'{p=}')
print(f'{q=}')
print(f'{c=}')
```

### out.txt
```
p=20322136122026329892580404875086132520732558134579258531781672192065024437324055172065343417524169304918928056147680414370351055409439818026607876517460045945556933456319117456860928521423787112252544266864178773974904640732880445449138842965327995838722222110164109025916914430044528254715080648900354468118393295346137198518513075775514617222780524163798065365970392865107270392212968677531885628998155305428785133820145555740608026626724539584106018453003156159305252013173659975815845286802275956807162426425721298560633326719023970391963404981189820163950120529861779878077006530640930032570206978446007206971761
q=19097560527100693557502945814016176943507375936656621847599300620729196257594977906326233653252987169303598004653720974045696589437233399711658994040877123702369987961301047714594623670674571987772814959679153558360152976652255742578324469478560556855210734037861198243000935281050776548747455717266013266531885744852759548255091579407464355390341944708706006878618904548103612995804547530724085856234186750409404880456083750984829553552127853848824218180459231650990529456828407224866655873224370892839628814748212142246752082561042142636866939231370987974125358875253454199574864895153300338298982667319003886687691
c=4281681357519343869235268029657832985104802601857889851833662824770073601279722389949102805423012693423900316266993146428480448851806951090530135683459342224839031144425810971344588481297094697047852347659595441639804230546879345999083627138617034295731725402645279785129174304818023129638779656619113578465655082808462489379872294929944719545647280271454196700396004152529288987570497804498041888697213294509916951489315431831556860863264254674452235360890586742441263188663158067860877772336480637257856658858967478284817730555629113613134338975168062044831796369552664256963808360408525644200922627703094455580032
```
<br/>

Normally, the public key used in RSA is a product of 2 prime numbers. In `chall.py`, a prime number `q` is used as the public key instead. We can then find the private key by calculating $d \equiv p^{-1} \pmod{\phi(q)}$ where $\phi (q) = q - 1$. We then find the original message `m` by calculating $m \equiv c^{d} \pmod{q}$ and convert it to bytes.

### Solve script
```Python3
from Crypto.Util.number import long_to_bytes

# Copied from out.txt
p=20322136122026329892580404875086132520732558134579258531781672192065024437324055172065343417524169304918928056147680414370351055409439818026607876517460045945556933456319117456860928521423787112252544266864178773974904640732880445449138842965327995838722222110164109025916914430044528254715080648900354468118393295346137198518513075775514617222780524163798065365970392865107270392212968677531885628998155305428785133820145555740608026626724539584106018453003156159305252013173659975815845286802275956807162426425721298560633326719023970391963404981189820163950120529861779878077006530640930032570206978446007206971761
q=19097560527100693557502945814016176943507375936656621847599300620729196257594977906326233653252987169303598004653720974045696589437233399711658994040877123702369987961301047714594623670674571987772814959679153558360152976652255742578324469478560556855210734037861198243000935281050776548747455717266013266531885744852759548255091579407464355390341944708706006878618904548103612995804547530724085856234186750409404880456083750984829553552127853848824218180459231650990529456828407224866655873224370892839628814748212142246752082561042142636866939231370987974125358875253454199574864895153300338298982667319003886687691
c=4281681357519343869235268029657832985104802601857889851833662824770073601279722389949102805423012693423900316266993146428480448851806951090530135683459342224839031144425810971344588481297094697047852347659595441639804230546879345999083627138617034295731725402645279785129174304818023129638779656619113578465655082808462489379872294929944719545647280271454196700396004152529288987570497804498041888697213294509916951489315431831556860863264254674452235360890586742441263188663158067860877772336480637257856658858967478284817730555629113613134338975168062044831796369552664256963808360408525644200922627703094455580032

# Private key
d = pow(p, -1, q - 1)
# Message
m = pow(c, d, q)
# Convert to bytes
print(long_to_bytes(m))
```

## Flag
```
wwf{ju57_u53_l1br4r135}
```