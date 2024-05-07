# LinAlgebra

## QR rozklad

Rozklad na:

- Ortonormální (unitární pro komplex. čísla) matice $Q$
    - Sloupce jsou navzájem ortogonální a mají jednotkovou normu
    - Platí: $Q^TQ = QQ^T = I$, kde $I$ je jednotková matice
- Horní trojúhelníkovou matici $R$

### Givensovy rotace

- Každá rotace **G** je orotogonální matice, která rotuje vektor ve dvourozměrném prostoru
- Na diagonálne vždy jedničky, kromě dvou prvků, které tvoří 2x2 rotační blok (velikost dle vstupní matice $A$)

$$
G = \begin{bmatrix}
cos(\phi) & -sin(\phi) \\
sin(\phi) & cos(\phi)
\end{bmatrix}
$$

- Každá z rotací nuluje jeden prvek (převod vektoru na vektor s úhlem 0&deg;)
- Postupně se vytvářejí rotace, které vynulují všechny prvky pod diagonálou

$$Q = I G_{1}^T G_{2}^T...G_{n-1}^T G_{n}^T $$

- A tedy platí:

$$ A = A_1$$

$$G_1 A_1 = A_2$$

$$...$$

$$G_n A_n = R$$

$$\Rightarrow G_n G_{n-1}...G_1 A = R$$


### Householderovy reflexe
- Iterativní proces pro každý sloupec $k$ matice $A$ od prvního do $n-1$ ($n$ je počet sloupců)
    - Vybere se podvektor $x$ od $k$-tého prvku do konce $k$-tého sloupce
    - Vytvoŕí se vektor $u$, který je roven $x$ s přidanou normou na první pozici $u = x + ||x||e_1$ ($e_1$ je jednotkový vektor s jedničkou na první pozici)
    - Vypočte se normalizovaný vektor $v$ jako $u$ dělené jeho normou $ v = \frac{u}{||u||}$
    - Sestaví se Householderova matice $P_k = I - 2vv^T$
    - Aplikuje se Householderova reflexe na A od $k$-tého sloupce, čímž se aktualizuje $A$ na $P_kAP_k$
- Po aplikaci Householderových reflexí je pak původní matice $A$ převedena na horní trojúhelníkovou matici $R$

$$ A = A_1$$

$$ A_1 P_1 = A_2$$

$$...$$

$$A_{n-1} G_{n-1} = R$$

$$\Rightarrow A H_1...H_{n-2}H{n-1} = R$$

- Matice $Q$ je výsledkem postupné aplikace všech $P_k$ ($Q = P_1 P_2 ... P_{n-2} P_{n-1}$)

### Gram-Schmidtův ortogonalizační proces

- Chceme nalézt ortogonální sloupce $q_1,q_2,...,q_n$ a koeficienty $r_{ij}$ takové, že

$$ A = Q \cdot R$$

- Proces:
    - Vezmeme první sloupec $a_1$ matice $A$
    - Vytvoříme první ortogonální vektor $q_1$ tak, že normalizujeme $a_1$
    - Pro následující sloupce vypočítáme projekci a orotogonalizaci
    - $v_i = a_i - \sum_{j=1}^{i-1}{proj_{q_j}(a_i)}$, kde $proj_{q_j}(a_i) = (q_j^T a_i)q_j$
    - Znormalizujeme $q_i  = \frac{v_i}{||v_i||}$
    - Výpočet matice R, kde koeficienty jsou dáno jako:
    - $r_{ij} = q_i^T a_j$ pro $j \geq i$ a $r_{ij} = 0$ pro $j < i$


### Výpočet vlastních čísel

- Inicializujeme $A_0 = A$
- Vypočítáme QR rozklad pro matice $A_k$
- Pronásobíme v opačném pořádí $A_{k+1} = R_kQ_k$
- Iterujeme postup do konvergence (matice A_k konverguje k diagonální formě, kde jsou na diagonále vlastní čísla)


## Singulární rozklad (SVG - Singular Value Decomposition)

- Rozkládá matici $A$ na tri specifické matice
1) $U$ - Ortogonální matice, jejíž sloupce jsou levé singulární vektory matice $A$
2) $Σ$ - Diagonální matice se singulárními hodnotami $𝜎_i$, které jsou nezápornéa seřazené v sestupném pořádí
3) $V^T$ - Ortogonální matice, jejíž řádky jsou pravé singulární vektory matice A

- Rozklad tedy je $A = UΣV^T$
    - $A$ - matice o rozměrech $m$ x $n$
    - $U$ - ortogonální matice o rozměrech $m$ x $m$
    - $Σ$ - diagonální matice o rozměrech $m$ x $n$
    - $V^T$ - ortogonální matice o rozměrech $n$ x $n$

Výpočet:
1) Výpočet matic $A^TA$ a $AA^T$
    - symetrické a pozitivné semidefinitní
2) Nalezení vlastních hodnot a vlastních vektorů
    - Vlastní čísla a vektory $A^TA$ udávají $V$ a singulární čísla Σ
    - Vlastní čísla a vektory $AA^T$ udávají U
3) Sestavení matice Σ
    - Diagonální prvky Σ jsou kladné druhé odmocniny vlastních čísel $A^TA$ (nebo $AA^T$) seřazené sestupně

Implementace:
 - Iterujeme přes řádky a sloupce, kdy cílem je z matice $A$ vytvořit horní bidiagonální matici B
 - Aplikujeme Householderovy reflexe pro sloupce (matice $U$) a pro řádky (matice $V$)
- Následně aplikujeme Golub - Kahanův algoritmus
    - Vytvoříme matici $T_0 = B^T B$
    - Iteračně na matici $T_k$ aplikujeme QR algoritmus, kdy čísla na diagonále konvergují k vlastním číslům matice $T_0$
- Odmocníme prvky na diagonále, čímž vzniknou singulární čísla vstupní matice $A$



