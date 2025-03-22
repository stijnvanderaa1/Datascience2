# Samenvatting: NumPy Indexing, Slicing, Ufuncs, Broadcasting, Boolean Masking en Fancy Indexing

## 1. Indexing
Met indexing selecteer je specifieke elementen uit een array op basis van hun index.  
De indexering begint bij **0** en negatieve indexen tellen vanaf het einde.

### Voorbeeld:
```python
import numpy as np

a = np.array([10, 20, 30, 40, 50])

# Eerste element
print(a[0])  # Output: 10

# Laatste element
print(a[-1]) # Output: 50

# Meerdimensionale array
b = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])

# Element in rij 2, kolom 3
print(b[1, 2])  # Output: 6

# Laatste rij
print(b[-1])  # Output: [7 8 9]

# eerste kolom
a[: ,0]

# eerste rij
a[0, :]
```
## 2. Slicing
Met slicing kun je delen van een array selecteren op basis van indexen.  
De basisstructuur is:  
```python
array[start:stop:step]
```

### Voorbeeld:
```python
import numpy as np

a = np.array([10, 20, 30, 40, 50])

# Eerste drie elementen
print(a[:3])   # Output: [10 20 30]

# Laatste twee elementen
print(a[-2:])  # Output: [40 50]

# Elke tweede element
print(a[::2])  # Output: [10 30 50]
```

### Meerdimensionale arrays:
```python
a = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])

# Eerste twee rijen
print(a[:2])
# Output:
# [[1 2 3]
#  [4 5 6]]

# Laatste kolom
print(a[:, -1])
# Output: [3 6 9]

# Hoekelementen (eigen selectie)
print(a[[0, 2], [0, 2]])
# Output: [1 9]


`[:, :3]` → Dit is slicing waarmee alleen de eerste drie kolommen worden geselecteerd:

- `:` → Neem alle rijen.  
- `:3` → Neem alleen de eerste drie kolommen (kolomindex 0, 1 en 2).  

```

---

## 3. Ufuncs (Universal Functions)
Ufuncs zijn ingebouwde, geoptimaliseerde functies voor elementgewijze operaties op arrays.  
Ze zijn veel sneller dan loops omdat ze in C zijn geïmplementeerd.

### Functies:
| Functie          | Beschrijving                     | Voorbeeld                                   |
|------------------|----------------------------------|--------------------------------------------|
| `np.add()`       | Elementgewijze optelling         | `np.add([1, 2], [3, 4]) → [4, 6]`         |
| `np.subtract()`  | Elementgewijze aftrekking        | `np.subtract([5, 3], [2, 1]) → [3, 2]`    |
| `np.multiply()`  | Elementgewijze vermenigvuldiging | `np.multiply([2, 3], [3, 4]) → [6, 12]`   |
| `np.divide()`    | Elementgewijze deling           | `np.divide([6, 4], [2, 2]) → [3, 2]`      |
| `np.sqrt()`      | Vierkantswortel                 | `np.sqrt([4, 9]) → [2, 3]`                |
| `np.sin()`       | Sinus van elk element           | `np.sin([0, np.pi/2]) → [0, 1]`           |

### Voorbeeld:
```python
a = np.array([1, 2, 3, 4])

# Optelling
result = np.add(a, 10)
print(result)  # Output: [11 12 13 14]

# Sinus
result = np.sin(a)
print(result)  # Output: [0.84147098 0.90929743 0.14112001 -0.7568025]
```

---
## 4 Aggregations
Aggregations zijn functies die een samenvatting geven van de waarden in een array, zoals het berekenen van de som, het gemiddelde of de standaardafwijking. Ze werken vaak sneller dan het handmatig itereren door de array.

### Veelgebruikte aggregatiefuncties:
| Functie              | Beschrijving                                   | Voorbeeld                        |
|---------------------|----------------------------------------------|----------------------------------|
| `np.sum()`          | Som van alle elementen                        | `np.sum([1, 2, 3]) → 6`         |
| `np.mean()`         | Gemiddelde van de elementen                   | `np.mean([1, 2, 3]) → 2.0`      |
| `np.median()`       | Mediaan van de elementen                      | `np.median([1, 2, 3]) → 2.0`    |
| `np.min()`          | Kleinste waarde                                | `np.min([1, 2, 3]) → 1`         |
| `np.max()`          | Grootste waarde                                | `np.max([1, 2, 3]) → 3`         |
| `np.std()`          | Standaardafwijking                             | `np.std([1, 2, 3]) → 0.816`     |
| `np.var()`          | Variantie                                      | `np.var([1, 2, 3]) → 0.666`     |

### Voorbeeld:
```python
import numpy as np

a = np.array([1, 2, 3, 4, 5])

# Som van de elementen
print(np.sum(a))  # Output: 15

# Gemiddelde
print(np.mean(a))  # Output: 3.0

# Mediaan
print(np.median(a))  # Output: 3.0

# Kleinste en grootste waarde
print(np.min(a), np.max(a))  # Output: 1 5

# Standaardafwijking en variantie
print(np.std(a), np.var(a))  # Output: 1.4142135623730951 2.0

# als je per  kolom dat wit berekenen doe je bv voor de standaardafwijking np.std(a, axis=0)
```
## 5. Broadcasting
Met broadcasting kun je arrays met verschillende afmetingen samen laten werken.  
Kleine arrays worden automatisch uitgebreid zodat de dimensies overeenkomen.

### Regels voor broadcasting:
1. Dimensies moeten overeenkomen van rechts naar links of gelijk zijn aan 1.
2. Als een dimensie niet overeenkomt en niet gelijk is aan 1 → Fout.
3. Dimensies met 1 worden automatisch gekopieerd over de ontbrekende as.

### Voorbeeld:
```python
a = np.array([1, 2, 3])      # Shape (3,)
b = np.array([[10], [20]])   # Shape (2, 1)

# Broadcasting maakt van a → [[1, 2, 3], [1, 2, 3]]
result = a + b
print(result)
# Output:
# [[11 12 13]
#  [21 22 23]]

# Eenvoudig voorbeeld:
a = np.array([1, 2, 3])
b = 10

# Broadcasting vult b aan zodat dimensies overeenkomen
print(a + b)  # Output: [11 12 13]
```

---

## 6. Boolean Masking
Met boolean masking kun je elementen uit een array selecteren op basis van een voorwaarde.

### Voorbeeld:
```python
a = np.array([10, 20, 30, 40, 50])

# Mask voor waarden groter dan 25
mask = a > 25
print(mask)  # Output: [False False  True  True  True]

# Alleen waarden > 25
result = a[mask]
print(result)  # Output: [30 40 50]

# Meerdere voorwaarden:
result = a[(a > 20) & (a < 50)]
print(result)  # Output: [30 40]

# Voorbeeld met een 2D-array:
a = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])

# Alle waarden > 5 uit de array
result = a[a > 5]
print(result)  # Output: [6 7 8 9]
```

---

## 7. Fancy Indexing
Met fancy indexing kun je specifieke rijen of kolommen uit een array selecteren op basis van een lijst met indexen.

### Voorbeeld:
```python
a = np.array([10, 20, 30, 40, 50])

# Selecteer elementen op specifieke indexen
indices = [0, 2, 4]
result = a[indices]
print(result)  # Output: [10 30 50]

# Fancy indexing in 2D-array:
a = np.array([[10, 20, 30], [40, 50, 60], [70, 80, 90]])

# Specifieke elementen op basis van rijen en kolommen
result = a[[0, 2], [1, 0]]
print(result)  # Output: [20 70]
```

---

## Overzicht
| Concept            | Beschrijving                                      | Voorbeeld                     |
|--------------------|--------------------------------------------------|-------------------------------|
| **Slicing**        | Selectie van elementen op basis van indexen       | `a[1:4] → [20, 30, 40]`       |
| **Ufuncs**         | Vectorized operaties over arrays                  | `np.add(a, b)`                |
| **Broadcasting**   | Arrays met verschillende vormen compatibel maken   | `a + b`                       |
| **Boolean Masking**| Selecteer elementen op basis van een voorwaarde   | `a[a > 10]`                   |
| **Fancy Indexing** | Selecteer elementen met een lijst van indexen     | `a[[0, 2, 4]]`                |
