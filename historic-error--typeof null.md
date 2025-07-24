### ❓ O que imprime `console.log(typeof null)`?

```javascript
console.log(typeof null); // "object"
```

**🧠 Mas por quê isso acontece, se `null` não é um objeto?**

* **Resposta curta:** Porque é um **bug histórico** do JavaScript.
* **Resposta longa:** Quando o JavaScript foi criado nos anos 90, os tipos eram representados internamente por códigos numéricos. O tipo "object" era identificado pelo código `0`. Por engano, o valor `null` também foi atribuído ao código `0`. Resultado: `typeof null` passou a retornar `"object"`.

Esse comportamento **não foi corrigido** para manter a compatibilidade com código antigo. Então, mesmo sendo confuso, ficou assim até hoje.

---

### ✅ Como verificar corretamente se algo é um objeto e **não** `null`?

Você precisa fazer **duas verificações**:

1. Verificar se o tipo é `"object"`.
2. Garantir que **não seja `null`**.

```javascript
const myVar = null;
console.log(typeof myVar === "object" && myVar !== null); // false

const anotherVar = {};
console.log(typeof anotherVar === "object" && anotherVar !== null); // true
```

---

### 🧩 Resumo:

| Valor         | `typeof` resultado |
| ------------- | ------------------ |
| `null`        | `"object"`         |
| `{}` (objeto) | `"object"`         |
| `[]` (array)  | `"object"`         |
| `123`         | `"number"`         |
| `"abc"`       | `"string"`         |

Então:
🟥 **`typeof null === "object"` é um erro histórico.**
🟩 Sempre combine com `!== null` se precisar saber se é realmente um objeto.

Se quiser, posso te mostrar um exemplo prático onde esse bug causa confusão.
