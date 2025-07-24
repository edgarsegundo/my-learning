Boa pergunta! O comportamento do **NaN** em JavaScript é realmente uma das coisas mais confusas e comentadas na linguagem — e não é à toa. Vou explicar o porquê disso existir, se é um erro, e para que serve.

---

### Por que existe o NaN em JavaScript?

O NaN (Not-a-Number) é um valor **especificado pelo padrão IEEE 754** para representar o resultado de operações matemáticas inválidas ou indefinidas, como:

* Dividir 0 por 0
* Converter uma string que não é um número (ex: `"hello"`) para número
* Raiz quadrada de número negativo (em JavaScript, retorna NaN)

Ou seja, o NaN não é um erro do JavaScript, mas sim uma representação padrão **de matemática computacional**, usada por muitas linguagens que adotam o padrão IEEE 754 para números de ponto flutuante.

---

### Por que `typeof NaN` é `"number"`?

Parece contraditório, mas não é exatamente um erro — é um efeito colateral do modelo de tipos do JavaScript:

* O NaN é um **valor numérico especial**, um subtipo dentro do tipo `number`.
* O operador `typeof` retorna tipos básicos do JavaScript: `number`, `string`, `object`, etc.
* Como NaN é definido como um valor numérico inválido, o `typeof` dele tem que ser `"number"` para seguir a especificação do ECMAScript.

**Se `typeof NaN` retornasse algo como `"NaN"` ou `"not-a-number"`, isso quebraria a consistência da linguagem e do modelo de tipos.**

---

### Por que NaN não é igual a ele mesmo (`NaN !== NaN`)?

Isso vem da especificação IEEE 754, que diz que NaN nunca é igual a outro valor, inclusive ele mesmo. Isso permite detectar erros em cálculos e sinais de que o resultado não é confiável.

---

### Isso é um erro no design do JavaScript?

Não é exatamente um erro, mas é um design que causa confusão para programadores, especialmente iniciantes. O JavaScript herdou muito da aritmética de ponto flutuante e das decisões do padrão IEEE 754.

Alguns aspectos são claramente “estranhos”, mas eles existem para:

* Manter compatibilidade com matemática numérica padrão
* Permitir operações que falhariam em outras linguagens
* Facilitar o manuseio de dados inválidos em cálculos

---

### Para que serve o NaN então?

* Para sinalizar resultados inválidos em cálculos numéricos
* Para indicar que um valor que deveria ser numérico não é um número válido
* Para ajudar na depuração, sinalizando erros silenciosos em operações matemáticas

---

### Dica prática: Como lidar com NaN?

* Use `Number.isNaN()` para testar estritamente se algo é NaN, sem coerção
* Não confie em comparações simples (`=== NaN` não funciona)
* Use validação e tratamento de erros para evitar que NaN se espalhe no seu código

---

### 🤔 Why is `NaN === NaN` **false**?

Because according to the **IEEE 754 standard** (which JavaScript uses for numbers), `NaN` is **defined as "not equal to any value, including itself"**.

This is intentional. Think of `NaN` as a marker that says:

> “I tried to compute a number, but the result is undefined or invalid. I don’t know what the value is — and neither should you.”

So if you compare two `NaN`s:

```js
NaN === NaN // false
```

You’re basically asking:

> "Is this unknown invalid number exactly equal to that other unknown invalid number?"

The answer is: **we don't know — they're both invalid**, so the comparison is not meaningful. Hence, it returns `false`.

---

### 🧪 Let’s look at your code:

```js
const valoresBrutos = ["100", "200", "erro", "300", "??", "150"];
const valoresConvertidos = valoresBrutos.map(valor => parseFloat(valor));

console.log(valoresConvertidos); 
// Output: [100, 200, NaN, 300, NaN, 150]

console.log(valoresConvertidos[2] === NaN); 
// false ❌
```

You expect it to return `true`, but it returns `false` — because `valoresConvertidos[2]` is `NaN`, and `NaN === NaN` is always false.

---

### ✅ Correct way to check for NaN

Use `Number.isNaN()` (introduced in ES6, and better than the old `isNaN()`):

```js
Number.isNaN(valoresConvertidos[2]) // true ✅
```

Or, the trick you saw earlier:

```js
valoresConvertidos[2] !== valoresConvertidos[2] // true ✅
```

Because `NaN` is the **only value in JavaScript** that is not equal to itself.

---

### 🔁 Summary

| Expression               | Result  | Why                            |
| ------------------------ | ------- | ------------------------------ |
| `NaN === NaN`            | `false` | NaN is never equal to anything |
| `Number.isNaN(NaN)`      | `true`  | Safest way to test for NaN     |
| `NaN !== NaN`            | `true`  | Unique behavior of NaN         |
| `val === val` (with NaN) | `false` | Only for NaN                   |

---

Ex:

```javascript
const valoresBrutos = ["100", "200", "erro", "300", "??", "150"];
const valoresConvertidos = valoresBrutos.map(valor => parseFloat(valor));

valoresConvertidos[2] === NaN // false

typeof NaN // 'number'

Number.isNaN() // false

Number.isNaN(valoresConvertidos[2]) // true
```
