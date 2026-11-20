# Exercici d'aprenentatge automàtic

## 1. Introducció

Aprenentatge automàtic (machine learning)

Idea central d'"aprendre"

* Ajustar de manera progressiva un conjunt de paràmetres interns (anomenats pesos, com w₁ i w₂) 
* Utilitzant dades d'entrada disponibles (variables característiques, x₁ i x₂).

Objectiu final:

- Aconseguir que la formulació matemàtica produeixi una predicció tan propera com sigui possible al resultat real conegut (y). 
- A cada intent o iteració, el model avalua la magnitud de l'error comès i utilitza aquesta informació per corregir la direcció dels pesos.
## 2. Plantejament de l'exercici: endevinar el preu d'una pizza
Imaginem que volem construir un model matemàtic senzill per predir el preu d'una pizza en funció de les seves característiques principals.

Variables d'entrada (característiques):

* x₁ = Diàmetre de la pizza (mesurat en pams, per exemple 1 o 2).
* x₂ = Quantitat d'ingredients extra (0, 1 o 2).

Fórmula inicial del model (sense terme independent o biaix):
* Predicció (ŷ) = w₁ · x₁ + w₂ · x₂

Trobar els millors valors numèrics per als pesos w₁ i w₂ mitjançant un procés intuïtiu de prova i error, mesurant la desviació respecte a les dades reals.

## 3. Taula de dades reals que volem encertar
Disposem d'una mostra de tres pizzes reals amb els seus respectius preus finals:

| Cas | x₁ (Diàmetre en pams) | x₂ (Extres) | Preu real (y) |
| :--- | :---: | :---: | :---: |
| Cas A | 1 | 1 | 10 € |
| Cas B | 2 | 0 | 14 € |
| Cas C | 2 | 2 | 18 € |

## 4. Procés d'entrenament per prova i error (Rondes 1 a 4)

### Ronda 1 — Comencem a ull (w₁ = 5, w₂ = 2)

Proposem una primera combinació de pesos de forma intuïtiva per veure com respon el model:

* Cas A: 5(1) + 2(1) = 7 € (error: 10 - 7 = +3 €)
* Cas B: 5(2) + 2(0) = 10 € (error: 14 - 10 = +4 €)
* Cas C: 5(2) + 2(2) = 14 € (error: 18 - 14 = +4 €)

Anem a pintar com a vectors:

* **Cas A:**
  $$\hat{y}_A = \begin{pmatrix} 5 \\ 2 \end{pmatrix} \cdot \begin{pmatrix} 1 \\ 1 \end{pmatrix} = 7\text{ €} \quad \longrightarrow \quad e_A = 10 - 7 = +3\text{ €}$$

* **Cas B:**
  $$\hat{y}_B = \begin{pmatrix} 5 \\ 2 \end{pmatrix} \cdot \begin{pmatrix} 2 \\ 0 \end{pmatrix} = 10\text{ €} \quad \longrightarrow \quad e_B = 14 - 10 = +4\text{ €}$$

* **Cas C:**
  $$\hat{y}_C = \begin{pmatrix} 5 \\ 2 \end{pmatrix} \cdot \begin{pmatrix} 2 \\ 2 \end{pmatrix} = 14\text{ €} \quad \longrightarrow \quad e_C = 18 - 14 = +4\text{ €}$$

Observació: En tots els casos la predicció queda per sota del valor real. Per tant, el model està infradimensionat i cal incrementar els valors de w₁ i/o w₂.

### Ronda 2 — Ajustem amb criteri (w₁ = 7, w₂ = 2)

Decidim augmentar el pes del diàmetre (w₁), ja que sembla l'element que més influeix en el cost final:

* **Cas A:**
  $$\hat{y}_A = \begin{pmatrix} 7 \\ 2 \end{pmatrix} \cdot \begin{pmatrix} 1 \\ 1 \end{pmatrix} = 9\text{ €} \quad \longrightarrow \quad e_A = 10 - 9 = +1\text{ €}$$

* **Cas B:**
  $$\hat{y}_B = \begin{pmatrix} 7 \\ 2 \end{pmatrix} \cdot \begin{pmatrix} 2 \\ 0 \end{pmatrix} = 14\text{ €} \quad \longrightarrow \quad e_B = 14 - 14 = 0\text{ € — clavat!}$$

* **Cas C:**
  $$\hat{y}_C = \begin{pmatrix} 7 \\ 2 \end{pmatrix} \cdot \begin{pmatrix} 2 \\ 2 \end{pmatrix} = 18\text{ €} \quad \longrightarrow \quad e_C = 18 - 18 = 0\text{ € — clavat!}$$

### Ronda 3 — Polim el detall (w₁ = 7, w₂ = 3)
Intentem eliminar l'error restant del Cas A augmentant lleugerament el pes dels ingredients extra (w₂):

* **Cas A:**
  $$\hat{y}_A = \begin{pmatrix} 7 \\ 3 \end{pmatrix} \cdot \begin{pmatrix} 1 \\ 1 \end{pmatrix} = 10\text{ €} \quad \longrightarrow \quad e_A = 10 - 10 = 0\text{ €}$$

* **Cas B:**
  $$\hat{y}_B = \begin{pmatrix} 7 \\ 3 \end{pmatrix} \cdot \begin{pmatrix} 2 \\ 0 \end{pmatrix} = 14\text{ €} \quad \longrightarrow \quad e_B = 14 - 14 = 0\text{ €}$$

* **Cas C:**
  $$\hat{y}_C = \begin{pmatrix} 7 \\ 3 \end{pmatrix} \cdot \begin{pmatrix} 2 \\ 2 \end{pmatrix} = 20\text{ €} \quad \longrightarrow \quad e_C = 18 - 20 = -2\text{ €}$$

Observació: En corregir el Cas A, hem provocat que el Cas C es passi de frenada. Això mostra que no sempre és possible aconseguir un error zero absolut a totes les dades utilitzant una fórmula lineal sense terme independent; cal cercar un equilibri que minimitzi l'error global.

## Ronda 4 — Ajust fi per repartir l'error (w₁ = 6,5, w₂ = 2,5)

Apliquem el concepte de taxa d'aprenentatge (learning rate), fent modificacions més petites i prudents en els pesos (de 0,5 en 0,5) per equilibrar el comportament:

* **Cas A:**
  $$\hat{y}_A = \begin{pmatrix} 6.5 \\ 2.5 \end{pmatrix} \cdot \begin{pmatrix} 1 \\ 1 \end{pmatrix} = 9\text{ €} \quad \longrightarrow \quad e_A = 10 - 9 = +1\text{ €}$$

* **Cas B:**
  $$\hat{y}_B = \begin{pmatrix} 6.5 \\ 2.5 \end{pmatrix} \cdot \begin{pmatrix} 2 \\ 0 \end{pmatrix} = 13\text{ €} \quad \longrightarrow \quad e_B = 14 - 13 = +1\text{ €}$$

* **Cas C:**
  $$\hat{y}_C = \begin{pmatrix} 6.5 \\ 2.5 \end{pmatrix} \cdot \begin{pmatrix} 2 \\ 2 \end{pmatrix} = 13 + 5 = 18\text{ €} \quad \longrightarrow \quad e_C = 18 - 18 = 0\text{ €}$$

## 5. Taula comparativa de les 4 rondes i conclusions
A continuació es resumeix l'evolució del model al llarg de les diferents proves:

| Ronda | w₁ | w₂ | Errors individuals (A, B, C) | Error absolut total |
| :--- | :---: | :---: | :---: | :---: |
| Ronda 1 | 5 | 2 | +3, +4, +4 | 11 € |
| Ronda 2 | 7 | 2 | +1, 0, 0 | 1 € |
| Ronda 3 | 7 | 3 | 0, 0, -2 | 2 € |
| Ronda 4 | 6,5 | 2,5 | +1, +1, 0 | 2 € |

### Lliçons clau de la comparativa

Avaluació de l'error global: Atenent únicament a l'error absolut total, la Ronda 2 (1 € d'error acomulat) és superior a la Ronda 4 (2 €). En un entorn d'entrenament real, l'algoritme detectaria que la modificació realitzada a la Ronda 4 ha empitjorat el resultat global i reduiria el pas d'ajust (per exemple, provant w₁ = 7, w₂ = 2,2).

Distribució dels errors i funció de pèrdua: 

* Observant errors individuals
  - Ronda 4 aconsegueix evitar desviacions grans com la de 2 € de la Ronda 3, 
  - Garantint que cap cas individual s'equivoqui de més d'1 €. 
* La selecció de la funció de pèrdua (loss function) 
  - determina si el model prefereix 
  - penalitzar més severament els errors puntuals grans 
  - o la suma total de petits errors.

## 6. Millora del model: afegir el biaix (bias) per aconseguir error zero

Per què no encaixava perfectament abans?

- Fins ara, el model no incloïa un terme independent. 
- Obligava la fórmula a assumir que si una pizza tenia mida 0 i 0 ingredients (x₁ = 0, x₂ = 0), -> preu havia de ser 0 €.

* En aprenentatge automàtic
  - Aquest terme addicional s'anomena *biaix* (bias, b o w₀) 
  - Representa el cost fix de base (massa, ús del forn, servei, caixa, etc.), 
  - Independentment de les caràcterístiques variables.

Nova fórmula del model: Predicció (ŷ) = w₁ · x₁ + w₂ · x₂ + b

### Ronda 5 — Provem d'afegir un biaix inicial (w₁ = 6, w₂ = 2, b = 1)

Provem d'afegir un terme independent (biaix) per intentar corregir la desviació del model:

* **Cas A:**
  $$\hat{y}_A = \begin{pmatrix} 6 \\ 2 \end{pmatrix} \cdot \begin{pmatrix} 1 \\ 1 \end{pmatrix} + 1 = 9\text{ €} \quad \longrightarrow \quad e_A = 10 - 9 = +1\text{ €}$$

* **Cas B:**
  $$\hat{y}_B = \begin{pmatrix} 6 \\ 2 \end{pmatrix} \cdot \begin{pmatrix} 2 \\ 0 \end{pmatrix} + 1 = 13\text{ €} \quad \longrightarrow \quad e_B = 14 - 13 = +1\text{ €}$$

* **Cas C:**
  $$\hat{y}_C = \begin{pmatrix} 6 \\ 2 \end{pmatrix} \cdot \begin{pmatrix} 2 \\ 2 \end{pmatrix} + 1 = 17\text{ €} \quad \longrightarrow \quad e_C = 18 - 17 = +1\text{ €}$$

Observació: Tots els casos es queden exactament 1 € per sota del preu real. Com que l'error és constant (+1 €) a tots tres casos, només cal pujar el biaix (b) en 1 € sense tocar els pesos!

### Ronda 6 — Ajustem només el biaix (w₁ = 6, w₂ = 2, b = 2)

Augmentem el valor del biaix en 1 unitat per compensar la desviació uniforme:

* **Cas A:**
  $$\hat{y}_A = \begin{pmatrix} 6 \\ 2 \end{pmatrix} \cdot \begin{pmatrix} 1 \\ 1 \end{pmatrix} + 2 = 10\text{ €} \quad \longrightarrow \quad e_A = 10 - 10 = 0\text{ €}$$

* **Cas B:**
  $$\hat{y}_B = \begin{pmatrix} 6 \\ 2 \end{pmatrix} \cdot \begin{pmatrix} 2 \\ 0 \end{pmatrix} + 2 = 14\text{ €} \quad \longrightarrow \quad e_B = 14 - 14 = 0\text{ €}$$

* **Cas C:**
  $$\hat{y}_C = \begin{pmatrix} 6 \\ 2 \end{pmatrix} \cdot \begin{pmatrix} 2 \\ 2 \end{pmatrix} + 2 = 18\text{ €} \quad \longrightarrow \quad e_C = 18 - 18 = 0\text{ €}$$

Observació: Error absolut total de 0 € (encert perfecte en tots els casos).
Taula comparativa de les rondes amb biaix

| Ronda | w₁ | w₂ | b | Errors individuals (A, B, C) | Error absolut total |
| :--- | :---: | :---: | :---: | :---: | :---: |
| Ronda 5 | 6 | 2 | 1 | +1, +1, +1 | 3 € |
| Ronda 6 | 6 | 2 | 2 | 0, 0, 0 | 0 € |

### 7. Deducció analítica dels paràmetres

Podem deduir l'estructura exacta del sistema comparant els casos entre si:

Càlcul de w₂ (pes dels extres): 

Si comparem el Cas B (2 pams, 0 extres → 14 €) amb el Cas C (2 pams, 2 extres → 18 €), veiem que afegir 2 extres augmenta el preu en 4 €. Per tant, cada extra té un valor fix de w₂ = 2 €.

Càlcul de w₁ (pes de la mida): 

Al Cas A (1 pam, 1 extra → 10 €), si descomptem els 2 € que aporta l'extra, obtenim que la mida de 1 pam té un valor assignat de 8 €. Passar de mida 1 (8 €) a mida 2 (14 € al Cas B) afegeix 6 € de diferència. Per tant, el pes del diàmetre és w₁ = 6 €.
Càlcul de b (biaix o cost base): 

Si la mida 1 apporta 6 € i no hi ha extres, el valor acumulat és 6 €. 
Com que el Cas A amb 1 pam i 1 extra (6 € + 2 €) suma 8 €, i el preu real és de 10 €, el cost fix base necessari és de b = 2 €.

## 8. Fase de predicció

Prenent els paràmetres òptims obtinguts a la **Ronda 6** ($\mathbf{w} = \begin{pmatrix} 6 \\ 2 \end{pmatrix}$ i $b = 2$), la funció de predicció del model és:

$$\hat{y} = 6x_1 + 2x_2 + 2$$

---

**1. Noves prediccions (Càlcul vectorial)**

* **Cas D — Pizza familiar (3 pams i 1 extra):** $\mathbf{x}_D = \begin{pmatrix} 3 \\ 1 \end{pmatrix}$
  $$\hat{y}_D = \begin{pmatrix} 6 \\ 2 \end{pmatrix} \cdot \begin{pmatrix} 3 \\ 1 \end{pmatrix} + 2 = 6(3) + 2(1) + 2 = \mathbf{22\text{ €}}$$

* **Cas E — Pizza mitjana (1.5 pams i 3 extres):** $\mathbf{x}_E = \begin{pmatrix} 1.5 \\ 3 \end{pmatrix}$
  $$\hat{y}_E = \begin{pmatrix} 6 \\ 2 \end{pmatrix} \cdot \begin{pmatrix} 1.5 \\ 3 \end{pmatrix} + 2 = 6(1.5) + 2(3) + 2 = \mathbf{17\text{ €}}$$

---


**3. Resum en taula**

| Cas | x₁ (Diàmetre en pams) | x₂ (Extres) | Preu predit (ŷ) |
| :--- | :---: | :---: | :---: |
| Cas D | 3 | 1 | 22 € |
| Cas E | 1.5 | 3 | 17 € |C

## Resum final 

* Els **pesos** (w₁, w₂): 
    - Regulen la sensibilitat o pendent del model. 
    - Indiquen quant varia la resposta del sistema quan canvia cadascuna de les variables d'entrada.

* El **biaix** (b): 
    - Desplaça la funció globalment cap amunt o cap avall en l'eix de sortida. 
    - Permet ajustar el punt de partida 
    - Sense forçar que la recta o pla hagi de passar necessàriament per l'origen de coordenades (0, 0).

