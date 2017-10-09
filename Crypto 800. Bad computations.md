
# Разбор задания Crypto 800. Bad computation

### Task
The creators of a certain system have taken care of the security of storing users data and encrypted users passwords. To register a new user the administrator should enter encrypted password into the database. You were able to get a users passwords database and a script, that administrator used for passwords encryption. The answer for this task is the password the encrypted version of which: hnd/goJ/e4h1foWDhYOFiIZ+f3l1e4R5iI+Gin+FhA==

### Code
Распишем что делает каждая функция


```python
from random import choice
from sys import argv
from base64 import b64encode, b64decode


b = 22

# возвращает простые числа от x до z включительно
def dwfregrgre(x, z):
    wdef = []
    for a in range(x, z + 1):
        for i in range(2, a):
            if (a % i) == 0:
                break
        else:
            wdef.append(a)

    return wdef

# возвращает простые числа до edefefef
def sdsd(edefefef):
    fvfegve = [x for x in range(2, edefefef)]

    x = 2
    rrerrrr = True
    while rrerrrr:
        for i in range(x * x, edefefef, x):
            if i in fvfegve:
                fvfegve.remove(i)

        rrerrrr = False
        for i in fvfegve:
            if i > x:
                x = i
                rrerrrr = True
                break

    return fvfegve


# расширенный алгоритм Евклида
# возвращает g, x, y такие, что a*x + b*y = g
def swsdwd(a, b):
    if a == 0:
        return (b, 0, 1)
    else:
        g, y, x = swsdwd(b % a, a)
        return (g, x - (b // a) * y, y)

# возвращает число d, обратное для a по модулю m
# то есть (d*a) mod m == 1
def swsdwdwdwa(a, m):
    g, x, y = swsdwd(a, m)
    if g != 1:
        raise Exception('Oops! Error!')
    else:
        return x % m

def L(u, n):
    return (u - 1) // n

```

#### Запишем нормально названия функций


```python
# Возвращает список простых чисел от x до z включительно
def get_prime_numbers_between(x, z):
    wdef = []
    for a in range(x, z + 1):
        for i in range(2, a):
            if (a % i) == 0:
                break
        else:
            wdef.append(a)

    return wdef


# возвращает простые числа до n
def get_prime_numbers_up_to(n):
    fvfegve = [x for x in range(2, n)]

    x = 2
    rrerrrr = True
    while rrerrrr:
        for i in range(x * x, n, x):
            if i in fvfegve:
                fvfegve.remove(i)

        rrerrrr = False
        for i in fvfegve:
            if i > x:
                x = i
                rrerrrr = True
                break

    return fvfegve

# расширенный алгоритм Евклида
# возвращает g, x, y такие, что a*x + b*y = g
def egcd(a, b):
    if a == 0:
        return (b, 0, 1)
    else:
        g, y, x = egcd(b % a, a)
        return (g, x - (b // a) * y, y)

# возвращает число d, обратное для a по модулю m
# то есть (d*a) mod m == 1
def get_inverse_by_mod(a, b):
    g, x, y = egcd(a, b)
    if g != 1:
        raise Exception('Oops! Error!')
    else:
        return x % b

def L(u, n):
    return (u - 1) // n
```

#### Посмотрим как работает алгоритм
Для наглядности заменим старые названия функций на новые


```python
def encrypt():
    print("Key cryptor v1.0")

    if len(argv) != 2:
        print("Start script like: python crypt.py <YourOwnPasswordString>")

    # Хочет, чтобы пароль был вида KLCRF{<something>}
    if (not str(argv[1]).startswith("KLCTF{")) or (not str(argv[1]).endswith("}")):
        print("Error! Password must starts with KLCTF")
        exit()

    # генерим массив случайный чисел от 100 до 1000 и выбираем случайно одно из них
    p = choice(get_prime_numbers_between(100, 1000))
    
    # генерим массив случайный чисел от 200 до 1000 и выбираем случайно одно из них
    q = choice(get_prime_numbers_between(200, 1000))

    print("Waiting for encryption...")

    n = p * q
    g = None
    
    # Если присмотреться, то видно, что i изменяется от n+1 до n^2
    # Внутри цикла хотим найти такое число, которое не делиться на p, q и на n.
    # Это условие можно упростить и проверять только делимость на n
    # (так как если число делится на n, то оно делится либо на p, либо на q)
    # Первое число из отрезка n+1 до n^2, которое не делится на n это n+1,
    # то есть g = n + 1 
    for i in range(n + 1, n * n):
        if ((i % p) == 0) or ((i % q) == 0) or ((i % n) == 0):
            continue

        g = i
        break
        
    # проверка бесполезная, g всегда существует
    if g is None:
        print("Error! Can't find g!")
        exit()

    # Здесь считаем функцию Эйлера для числа n
    # φ(n) = φ(p)*φ(q) = (p-1)(q-1) (так как p и q по условию простые)
    lamb = (p - 1) * (q - 1)
    
    # Этот момент отдельно разобрат внизу в пункте 1.
    # Если кратко, то эту строку можно заменить на
    # mu = get_inverse_by_mod(lamb, n)%n
    mu = get_inverse_by_mod(L(pow(g, lamb, n * n), n), n) % n

    # Возвращает список простых чисел до n-1
    rc = get_prime_numbers_up_to(n - 1)
    
    
    # проверка бесполезная, rc всегда имеет не нулевую длину(n всегда больше 101^2)
    if len(rc) == 0:
        print("Error! Candidates for r not found!")
        exit()

    # убираем из rc числа p и q
    if p in rc:
        rc.remove(p)
    if q in rc:
        rc.remove(q)

    # берем одно из них наугад
    r = choice(rc)

    # переводим наш исходный пароль в лист интов: ord(x) возвращает ascii код x
    password_chars = [ord(x) for x in argv[1][6:-1]]
    
    
    # Всю эту магию можно заменить на password_chars[i] = password_chars[i] + b
    # Подробнее об этом в пункте 2 ниже
    dcew = (pow(g, b, (n * n)) * pow(r, n, (n * n))) % (n * n)
    for i in range(len(wdwfewgwggrgrg)):
        password_chars[i] = (((pow(g, password_chars[i], (n * n)) * pow(r, n, (n * n))) % (n * n)) * dcew) % (n * n)
        password_chars[i] = (L(pow(password_chars[i], lamb, (n * n)), n) * mu) % n

    #преобразовываем в байты, далее печатаем
    ecrypted = b64encode(bytearray(password_chars))
    print(str(wdwfewgwggrgrg)[2:-1])

```

### Здесь будет немного математики!
#### 1. Упрощение mu = get_inverse_by_mod(L(pow(g, lamb, n * n), n), n) % n

$g = n+1 = p*q + 1$  
  
$ g^{lamb}\pmod{n^{2}}= (n+1)^{lamb}\pmod{n^2}$   
  
Посчитаем $g^{2} \pmod{n^2}:$  
$ g^{2}\pmod{n^{2}} = (n^{2} + 2*n + 1)\pmod{n^{2}} = (2*n + 1) \pmod {n^2}$  
  
Посчитаем $g^{3} \pmod{n^2}:$  
$ g^{3}\pmod{n^{2}} = (g^{2}\pmod{n^{2}} )* ((n+1)\pmod{n^2}) = (2*n^2 + 3*n + 1)\pmod{n^2} = (3*n + 1)\pmod{n^2}$  
  
Посчитаем $g^4\pmod{n^2}:$  
$ g^{4}\pmod{n^{2}} = (g^{3}\pmod{n^{2}} )* ((n+1)\pmod{n^2}) = (3*n^2 + 4*n + 1)\pmod{n^2} = (4*n + 1)\pmod{n^2}$  
  
Посчитаем $g^{lamb}\pmod{n^2}:$  
$ g^{lamb}\pmod{n^{2}} = (lamb * n + 1)\pmod{n^2}$  
  
    
     
Теперь применим к этому функцию $L$:  
$L(lamb*n + 1, n) = ((lamb*n + 1) - 1)//n = lamb$
  
Таким образом, $mu = get\_inverse\_by\_mod(lamb, n)\pmod n$, то есть это просто обратное число к $lamb$ по модулю $n$

#### 2. Упрощение цикла
```python
dcew = (pow(g, b, (n * n)) * pow(r, n, (n * n))) % (n * n)  
for i in range(len(wdwfewgwggrgrg)):  
    password_chars[i] = (((pow(g, password_chars[i], (n * n)) * pow(r, n, (n * n))) % (n * n)) * dcew) % (n * n)   
    password_chars[i] = (L(pow(password_chars[i], lamb, (n * n)), n) * mu) % n  
```

Пусть для простоты password_chars[i] = s  
$dcew = ((g^b \pmod{n^2}) * (r^n \pmod{n^2})) \pmod{n^2}$  
  
В цикле мы умножаем $dcew$ на $g^s\pmod{n^2}) * (r^n \pmod{n^2})) \pmod{n^2}$:  
$s = dcew * (g^s\pmod{n^2}) * (r^n \pmod{n^2})) \pmod{n^2}) \pmod{n^2} = (g^{s+b}\pmod{n^2}) * (r^{2n} \pmod{n^2}) \pmod{n^2}$  
  
 
Перейдем теперь ко второй строке тела цикла:  
$s = (L(s^{lamb}\pmod{n^2}, n)* mu) \pmod n = (L((g^{(s+b)*lamb}\pmod{n^2}) * (r^{2n*lamb} \pmod{n^2}) \pmod{n^2}), n) * mu)\pmod n$.  
  
Посмотрим внимательно на $r^{2n*lamb} \pmod{n^2}$:  
Вспомним свойство функции Эйлера: $a^{\phi(m)}\pmod m == 1$, если $а$ и $m$ взаимнопростые.  
$lamb = \phi(n)$, то есть $r^{lamb} \pmod n == 1$, но у нас $n^2$, а не $n$. ($r$ и $n$ взаимнопростые, так как $n = p*q$, а из $r$ мы явно убирали $p$ и $q$) 
  
Докажем, что если $r \pmod n == 1$, то $r^n \pmod{n^2} == 1$:  
Пусть $r = a*n + b$, где $a$ какое-то целое неотрицательное число, а $b = 1$ по условию.  
Тогда, воспользовавшись приемом из пункта 1, получаем:  
$r^n \pmod {n^2} = (n^2*a*b^3 + b^4 ) \pmod {n^2} = b^4 = 1^4 = 1$
  
Из этого следует, что $s = (L((g^{(s+b)*lamb}\pmod{n^2}) * 1^{2}) \pmod{n^2}), n) * mu)\pmod n$
  
Теперь распишем $g^{(s+b)*lamb}\pmod {n^2}$:  
$g^{(s+b)*lamb} \pmod{n^2} = (lamb * (s+b) * n + 1) \pmod {n^2}$ (см. пункт 1)  
  
Тогда $s = (L(((s+b)*lamb*n + 1), n) * mu)\pmod n = ( ( ( (s+b)*lamb*n + 1) - 1 )// n) * mu) \pmod n = (lamb *(s+b) *mu) \pmod n$  
  
Вспомним, что $mu$ является обратным числом к $lamb$ по модулю $n$, то есть $(mu * lamb) \pmod n == 1)$,   
то есть $s = s + b$.
Получается, что все шифрование заключается исключительно в сдвиге на $b$

### Заимплементим код расшифровки:


```python
def decrypt(encrypted):
    encrypted_bytes = list(b64decode(encrypted))
    decrypted_bytes = []
    for byte in encrypted_bytes:
        decrypted_bytes.append(byte - 22)
    decrypted_password = ''.join(map(chr, decrypted_bytes))
    print(decrypted_password)
```


```python
encrypted = "hnd/goJ/e4h1foWDhYOFiIZ+f3l1e4R5iI+Gin+FhA=="
decrypt(encrypted)
```

    paillier_homomorphic_encryption


KLCTF{paillier_homomorphic_encryption}
